#!/usr/bin/perl -T

#------------------------------------------------------------------------------
# This is amavisd-signer, a DKIM signing service daemon for amavisd.
# It uses an AM.PDP protocol lookalike to receive a request from amavisd
# and provides two services: choosing a signing key, and signing a
# message digest with a chosen DKIM private key.
#
# Author: Mark Martinec <Mark.Martinec@ijs.si>
#
# Copyright (c) 2010-2014, Mark Martinec
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions
# are met:
# 1. Redistributions of source code must retain the above copyright notice,
#    this list of conditions and the following disclaimer.
# 2. Redistributions in binary form must reproduce the above copyright notice,
#    this list of conditions and the following disclaimer in the documentation
#    and/or other materials provided with the distribution.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
# AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
# ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS
# BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
# CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
# SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
# INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
# CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
# ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
# POSSIBILITY OF SUCH DAMAGE.
#
# The views and conclusions contained in the software and documentation are
# those of the authors and should not be interpreted as representing official
# policies, either expressed or implied, of the Jozef Stefan Institute.

# (the above license is the 2-clause BSD license, also known as
#  a "Simplified BSD License", and pertains to this program only)
#
# Patches and problem reports are welcome.
# The latest version of this program is available at:
#   http://www.ijs.si/software/amavisd/
#------------------------------------------------------------------------------

# Using a separate signing service (which may run under a dedicated UID or
# GID or as root, having exclusive access to private keys) releaves amavisd
# process from needing to have access to private keys. Separating roles can
# provide improved protection for DKIM private keys, and/or can provide more
# flexibility in choosing a signing key.
#
# Usage:
#   amavisd-signer &

package AmavisSigner;

use strict;
use re 'taint';
use warnings FATAL => 'utf8';
no warnings 'uninitialized';

use Sys::Syslog;  # used by Net::Server for logging
use MIME::Base64;
use Mail::DKIM;
use Mail::DKIM::PrivateKey;

use Storable qw(thaw freeze nfreeze);
use DBI;
require 'dumpvar.pl';

use Net::Server 0.91;
use Net::Server::Multiplex;
use vars qw(@ISA);
@ISA = qw(Net::Server::Multiplex);

use vars qw(
  $VERSION $log_level
  %dkim_signing_keys_by_domain
  @dkim_signing_keys_list @dkim_signing_keys_storage
  @dkim_signature_options_bysender_maps
  $daemon_chroot_dir $daemon_user $daemon_group $pid_file $daemonize
  $inet_socket_bind @listen_sockets $listen_queue_size
  $syslog_ident $syslog_facility
);

$VERSION = 1.001;  # 20100730

#
# Please adjust the following settings as necessary:
#

$daemon_user  = 'vscan';
$daemon_group = 'vscan';
# $daemon_chroot_dir = '/var/amavis';   # chroot directory or undef

# $daemonize = 1;

$log_level = 2;  # 0..5
$syslog_facility = 'mail';
$syslog_ident = 'amavisd-signer';

# the $inet_socket_bind and @listen_sockets should correspond to a
# setting $dkim_signing_service in amavisd.conf :
$inet_socket_bind = '127.0.0.1';
@listen_sockets = ( 20203 );
$listen_queue_size = undef;  # uses a default

# mysql connection parameters
#
my $SQLdatabase="dkim_keys";
my $SQLhostname="192.168.254.50";
my $SQLport=3306;
my $SQLuser="root";
my $SQLpassword="";
my $dsn = "DBI:mysql:database=$SQLdatabase;host=$SQLhostname;port=$SQLport";

# Load all available private keys and supply their public key RR constraints.
# Arguments are a domain, a selector, a key (a file name of a private key in
# PEM format), followed by optional attributes/constraints (tags, represented
# here as Perl hash key/value pairs) which are allowed by RFC 4871 in a public
# key resource record (v, g, h, k, n, s, t), of which only g, h, k, s and t
# are considered to be constraints limiting the choice of a signing key.
#
#         signing domain   selector     private key              options
#          -------------   --------     ----------------------   ----------
# dkim_key('example.org', 'abc',       '/var/db/dkim/a.key.pem');
# dkim_key('example.org', 'yyy',       '/var/db/dkim/b.key.pem', t=>'s');
# dkim_key('example.org', 'zzz',       '/var/db/dkim/b.key.pem', h=>'sha256');
# dkim_key('example.com', 'sel-2008',  '/var/db/dkim/sel-example-com.key.pem',
#          t=>'s:y', g=>'*', k=>'rsa', h=>'sha256:sha1', s=>'email',
#          n=>'testing; 1, 2');
# dkim_key('guest.example.com', 'g',    '/var/db/dkim/g-guest-ex-com.key.pem');
# dkim_key('mail.example.com', 'notif', '/var/db/dkim/notif-mail.key.pem');

# @dkim_signature_options_bysender_maps maps author/sender addresses or
# domains to signature tags/requirements; possible signature tags according
# to RFC 4871 are: (v), a, (b), (bh), c, d, (h), i, l, q, s, (t), x, z;
# of which the following are determined implicitly: v, b, bh, h, t
# (tag h is controlled by %signed_header_fields);  currently ignored tags
# are l and z;  instead of an absolute expiration time (tag x) one may use
# a pseudo tag 'ttl' to specify a relative expiration time in seconds, which
# is converted to an absolute expiration time prior to signing: x = t + ttl;
# a built-in default is provided for each tag if no better match is found
#
# @dkim_signature_options_bysender_maps = ( {
#   'postmaster@mail.example.com' => { a => 'rsa-sha1', ttl =>  7*24*3600 },
#   'spam-reporter@example.com'   => { a => 'rsa-sha1', ttl =>  7*24*3600 },
#   'mail.example.com'            => { a => 'rsa-sha1', ttl => 10*24*3600 },
#   # explicit 'd' forces a third-party signature on foreign (hosted) domains
#   'guest.example'               => { d => 'guest.example.com' },
#   '.example.com'                => { d => 'example.com' },
#   # catchall defaults
#   '.' => { a => 'rsa-sha256', c => 'relaxed/simple', ttl => 30*24*3600 },
#   # 'd' defaults to a domain of an author/sender address,
#   # 's' defaults to whatever selector is offered by a matching key
# } );


#
# No further user-configurable settings below (but feel free
# to customize code in choose_key_request() or replace it altogether.
#

sub ll($) {
  my($level) = @_;
  $level <= $log_level;
}

my($server);  # a Net::Server object
sub do_log($$;@) {
  my($level, $errmsg, @args) = @_;
  $errmsg = sprintf($errmsg,@args)  if @args;
  if ($level <= $log_level) {
    my($prio);  # Net::Server logging priority
    # 0=err, 1=warning, 2=notice, 3=info, 4=debug
    if    ($level >=  3) { $prio = 4 }
    elsif ($level >=  0) { $prio = 2 }
    elsif ($level >= -1) { $prio = 1 }
    else                 { $prio = 0 }
    $server->log($prio, sanitize_str($errmsg));
    # Net::Server directs STDERR to the log_file
    # print STDERR sanitize_str($errmsg)."\n";
  }
}

sub sanitize_str {
  my($str, $keep_eol) = @_;
  my(%map) = ("\r" => '\\r', "\n" => '\\n', "\f" => '\\f', "\t" => '\\t',
              "\b" => '\\b', "\e" => '\\e', "\\" => '\\\\');
  if ($keep_eol) {
    $str =~ s/([^\012\040-\133\135-\176])/  # and \240-\376 ?
              exists($map{$1}) ? $map{$1} :
                     sprintf(ord($1)>255 ? '\\x{%04x}' : '\\%03o', ord($1))/eg;
  } else {
    $str =~ s/([^\040-\133\135-\176])/      # and \240-\376 ?
              exists($map{$1}) ? $map{$1} :
                     sprintf(ord($1)>255 ? '\\x{%04x}' : '\\%03o', ord($1))/eg;
  }
  $str;
}

sub split_address($) {
  my($mailbox) = @_;  local($1,$2);
  $mailbox =~ /^ (.*?) ( \@ (?:  \[  (?: \\. | [^\]\\] ){0,999} (?: \] | \z)
                              |  [^\[\@] )*
                       ) \z/xs ? ($1, $2) : ($mailbox, '');
}

#
#
#
sub dkim_key($$$;@) {
  my($domain,$selector,$key) = @_;  shift; shift; shift;
  @_%2 == 0 or die "dkim_key: a list of key/value pairs expected as options\n";
  my(%key_options) = @_;  # remaining args are options from a public key RR
   
  my $query = "";
  my @data ;
 
  my $connection = DBI->connect($dsn,$SQLuser,$SQLpassword,{RaiseError => 1, mysql_auto_reconnect => 1});   
  $connection->{RaiseError} = 1; # error handling enabled
  $connection->{AutoCommit} = 0; # transaction enabled 
  
  defined $domain && $domain ne ''
    or die "dkim_key: domain must not be empty: ($domain,$selector,$key)";
  defined $selector && $selector ne ''
    or die "dkim_key: selector must not be empty: ($domain,$selector,$key)";
  my $key_storage_ind ;
  
 eval { 
  if (ref $key) {  # key already preprocessed and provided as an object
      # insert key, 
      $query = "insert into private_keys ( key_object, key_pem, key_file_name) values (?, ?, ?)";
      $statement = $connection->prepare($query);
	  my $binkey = nfreeze $key;
	  $statement->execute($binkey,"not set","not set");
	  
	  $statement->prepare('SELECT LAST_INSERT_ID()');
	  $statement->execute();
	  @data = $statement->fetchrow_array();
	  $key_storage_ind = $data[0];
      # push(@dkim_signing_keys_storage, [$key]);
      # $key_storage_ind = $#dkim_signing_keys_storage;
  } else {  # assume a name of a file containing a private key in PEM format
    my($fname) = $key;
    my($pem_fh) = IO::File->new;  # open a file with a private key
    $pem_fh->open($fname,'<') or die "Can't open PEM file $fname: $!";
    my(@stat_list) = stat($pem_fh);  # soft-link friendly
    @stat_list or warn "Error on accessing $fname: $!";
    # does exist the key in DB ?
	$statement = $connection->prepare('select `key_storage_ind` from private_keys where `key_file_name` = "'.$fname.'"');
	$statement->execute();
	if ($statement->rows gt 0) {
	  @data = $statement->fetchrow_array();
	  $key_storage_ind = $data[0]; # the first key should be the one.
	} 
    if (!defined($key_storage_ind)) {
      # read file and store its contents as a new entry
      my($nbytes,$buff); $key = '';
      while (($nbytes=$pem_fh->read($buff,16384)) > 0) { $key .= $buff }
      defined $nbytes or die "Error reading key from file $fname: $!";
      # lets store the key
	  
	  my $key_object = Crypt::OpenSSL::RSA->new_private_key($key);
	  	  
	  $query = "insert into private_keys (`key_pem`,`key_object`,`key_file_name` ) values ( ?,?,?)"; 
	  $statement= $connection->prepare($query);
	  $statement->execute($key,nfreeze($key_object),$fname);
	  $query = "SELECT LAST_INSERT_ID();";
	  $statement = $connection->prepare($query);
	  $statement->execute();
	  @data = $statement->fetchrow_array();
	  $key_storage_ind = $data[0]; # the first key should be the one.	  
	  # push(@dkim_signing_keys_storage, [$key,$dev,$inode,$fname]);
      # $key_storage_ind = $#dkim_signing_keys_storage;
    }
    $pem_fh->close or die "Error closing file $fname: $!";
    $key_options{k} = 'rsa'  if defined $key_options{k};  # force RSA
  }
  $domain   = lc($domain)  if !ref($domain);  # possibly a regexp
  $selector = lc($selector);
  $key_options{domain} = $domain; $key_options{selector} = $selector;
  $key_options{key_storage_ind} = $key_storage_ind;
  $key_options{v} = 'DKIM1'  if !defined $key_options{v};  # provide a default
  if (defined $key_options{n}) {  # encode n as qp-section (rfc4871, rfc2047)
	$key_options{n} =~ s{([\000-\037\177=;"])}{sprintf('=%02X',ord($1))}egs;
  }
  $statement = $connection->prepare("select * from key_defs where `domain` like \"".$connection->quote($key_options{domain})."\" and `selector` like \"".$key_options{selector}."\"");
  $statement->execute();
  
  if ($statement->rows gt 0) { die "dkim_key: selector $selector for domain $domain already in use\n"; }
  $query = "insert into key_defs (`domain`,`key_storage_ind`,`selector`,`k`,`v`,`t`,`s`,`n`,`h`,`g`) values (?,?,?,?,?,?,?,?,?,?)";
  $statement = $connection->prepare($query);
  $statement->execute($connection->quote($key_options{domain}),$key_options{key_storage_ind} ,$key_options{selector},$key_options{k},$key_options{v},$key_options{t},$key_options{s},$key_options{n},$key_options{h},$key_options{g});
  #push(@dkim_signing_keys_list, \%key_options);  # using a list preserves order

  $query = "SELECT LAST_INSERT_ID();";
  $statement = $connection->prepare($query);
  $statement->execute();
  @data = $statement->fetchrow_array();
  my $j = $data[0];  
  my($any_wild);
  if (ref($domain) eq 'Regexp') {
	  $key_options{domain_re} = $domain;
	  $any_wild = sprintf("key#%d, %s", $j, $domain)  if !defined $any_wild;
  } elsif ($domain =~ /\*/) {
	  # wildcarded signing domain in a key declaration, evil, asks for trouble!
	  # support wildcards in signing domain for compatibility with dkim_milter
	  my($regexp) = $domain;
	  $regexp =~ s/\*{2,}/*/gs;   # collapse successive wildcards
	  # '*' is a wildcard, quote the rest
	  $regexp =~ s{ ([@#/.^$|*+?(){}\[\]\\]) }{$1 eq '*' ? '.*' : '\\'.$1}gex;
	  $regexp = '^' . $regexp . '\\z';  # implicit anchors
	  $regexp =~ s/^\^\.\*//s;    # remove leading anchor if redundant
	  $regexp =~ s/\.\*\\z\z//s;  # remove trailing anchor if redundant
	  $regexp = '(?:)'  if $regexp eq '';  # just in case, non-empty regexp
	  # presence of {'domain_re'} entry lets get_dkim_key use this regexp
	  # instead of a direct string comparision with {'domain'}
	  $key_options{domain_re} = qr{$regexp};  # compiled regexp object
	  $any_wild = sprintf("key#%d, %s", $j, $domain)  if !defined $any_wild;
  }
  if ( $key_options{domain_re} ) { 
     $connection->do("update `key_defs` set `domain`= \"REGEXP\", `domain_re` = \"$key_options{domain_re}\" where `key_ind` = $j");
  }
  if (!defined($key_options{domain_re})) { # no regexp, just plain match on domain
      $statement = $connection->prepare("select * from domains where `domain` like \"$domain\" and `keydef_link` = $j ");  
	  $statement->execute(); # is the domains / key pair already stored in the domains ?
	  
	  my @data = $statement->fetchrow_array();
	   print "Adding PLAIN $domain, $j, ".$statement->rows."\n";	
      if ($statement->rows eq 0 ) {  # if it is not then 
		  $connection->do("insert into domains (`domain`, `keydef_link` ) values (\"$domain\",$j )"); # store it
		  $statement = $connection->prepare("select * from domains where `domain` like \"WILD\" ");    # however is  
		  $statement->execute();         # any wildcarded domain registered already?
		  
		  $connection->{RaiseError} = 0; # this is potentially with error.
		  while (@data = $statement->fetchrow_array()) { # then add into currect record also refferences to wildcarded keys.
		     $connection->do("insert into domains (`domain`,`keydef_link`) values (\"$domain\",$data[1])");
			 print "Adding Wildcard link to PLAIN $domain, $j, ".$statement->rows."\n";
	      }
          $connection->{RaiseError} = 1; # this is potentially with error.
		  
	  }
	  
  } else {  # a wildcard in a signing domain, compatibility with dkim_milter
	  # wildcarded signing domain potentially matches any _by_domain entry
	  $statement = $connection->prepare("select `domain` from domains group by `domain`");
	  $statement->execute();
	  while (@data = $statement->fetchrow_array()) {
		($data[0] !~ /^WILD$/ )&& $connection->do("insert into domains (`domain`,`keydef_link`) values (\"$data[0]\",$j)");
	  }
	  # the '*' entry collects only wildcarded signing keys
	  $connection->do("insert into domains (`domain`,`keydef_link`) values (\"WILD\",$j)");
	  printf("dkim: wildcard in signing domain (%s), may produce unverifiable ".
        "signatures with no published public key, avoid!\n", $any_wild)
      if $any_wild;
  }
  $connection->commit();
 };
 if($@){
	 $connection->rollback();
	 print "Error inserting key: $@";
 }
 $connection->disconnect();
 
}

sub dkim_key_postprocess() {
  # convert private keys (as strings in PEM format) into RSA objects
  # die "Somebody called dkim_key_postprocess() but this is deprecated.\n";
}

# THE get_dkim_key IS A DIRECT COPY OF THE SAME ROUTINE FROM amavisd
#
# Fetch a private DKIM signing key for a given signing domain, with its
# resource-record (RR) constraints compatible with proposed signature options.
# The first such key is returned as a hash; if no key is found an empty hash
# is returned. When a selector (s) is given it must match the selector of
# a key; when algorithm (a) is given, the key type and a hash algorithm must
# match the desired use too; the service type (s) must be 'email' or '*';
# when identity (i) is given it must match the granularity (g) of a key;
#
# sign.opts.     key options
# ----------     -----------
#  d         =>  domain
#  s         =>  selector
#  a         =>  k, h(list)
#  i         =>  g, t=s
#
sub get_dkim_key(@) {
  @_ % 2 == 0 or die "get_dkim_key: a list of pairs is expected as query opts";
  my(%options) = @_;  # signature options (v, a, c, d, h, i, l, q, s, t, x, z),
    # of which d is required, while s, a and t are optional but taken into
    # account in searching for a compatible key - the rest are ignored
  my(%key_options);
  my($domain) = $options{d};
  my $connection = DBI->connect($dsn,$SQLuser,$SQLpassword,{RaiseError => 1, mysql_auto_reconnect => 1});
  my($statement);
  
  defined $domain && $domain ne ''
    or die "get_dkim_key: domain is required, but tag 'd' is missing";
  $domain = lc($domain);
  my(@indices) = ();
  
  #
  #  Search for key candidates:
  #
  $statement = $connection->prepare("select `keydef_link` from domains where `domain` like \"$domain\""); # specific candidate with $domain = domain from domains ...	   
  $statement->execute();
  if ($statement->rows gt 0) {
      while (my $indice = $statement->fetchrow_array()) { 
	     push @indices,$indice[0]; }
  } else { # universal candidate from domains where domain = "WILD"
	  $statement = $connection->prepare("select `keydef_link` from domains where `domain` like \"WILD\"");				   
	  $statement->execute();
	  if ($statement->rows gt 0) { 
	     while (my $indice = $statement->fetchrow_array()) { 
		     push @indices,$indice[0]; }}
  } 
  
  if (@indices) {
 
    my($selector) = $options{s};
    $selector = $selector eq '' ? undef : lc($selector)  if defined $selector;
    local($1,$2);
    my($keytype,$hashalg) =
      defined $options{a} && $options{a} =~ /^([a-z0-9]+)-(.*)\z/is ? ($1,$2)
                                                              : ('rsa',undef);
    my($identity_localpart,$identity_domain) =
      !defined($options{i}) ? () : split_address($options{i});
    $identity_localpart = ''  if !defined $identity_localpart;
    $identity_domain    = ''  if !defined $identity_domain;
    # find the first key (associated with a domain) with compatible options
    for my $j (@indices) {
	  $statement = $connection->prepare("select * from key_defs where `key_ind` = $j "); $statement->execute(); 
      # my($ent) = $dkim_signing_keys_list[$j];
	  my($ent) = $statement->fetchrow_hashref();
      next unless defined $ent->{domain_re} ? $domain =~ $ent->{domain_re}
                                            : $domain eq $ent->{domain};
      next if defined $selector && $ent->{selector} ne $selector;
      next if $keytype ne (exists $ent->{k} ? $ent->{k} : 'rsa');
      next if exists $ent->{s} &&
              !(grep { $_ eq '*' || $_ eq 'email' } split(/:/, $ent->{s}) );
      next if defined $hashalg && exists $ent->{'h'} &&
              !(grep { $_ eq $hashalg } split(/:/, $ent->{'h'}) );
      if (defined($options{i})) {
        if (lc($identity_domain) eq $domain) {
          # ok
        } elsif (exists $ent->{t} && (grep {$_ eq 's'} split(/:/,$ent->{t}))) {
          next;  # no subdomains allowed
        }
        if (!exists($ent->{g}) || $ent->{g} eq '*') {
          # ok
        } elsif ($ent->{g} =~ /^ ([^*]*) \* (.*) \z/xs) {
          next if $identity_localpart !~ /^ \Q$1\E .* \Q$2\E \z/xs;
        } else {
          next if $identity_localpart ne $ent->{g};
        }
      }
      %key_options = %$ent;  last;  # found a suitable match
    }
  }
  if (defined $key_options{key_storage_ind}) {
       # obtain actual key from @dkim_signing_keys_storage
	$statement = $connection->prepare("select key_object from private_keys where `key_storage_ind` = $key_options{key_storage_ind}"); $statement->execute(); 
	 my @bytestream = $statement->fetchrow_array();
    ($key_options{key}) = thaw $bytestream[0];
      @{$dkim_signing_keys_storage[$key_options{key_storage_ind}]};
  }
	print '%key_options'."\n";
	dumpValue(\%key_options);
  $connection->disconnect();
  %key_options;
}

sub proto_encode($@) {
  my($attribute_name,@strings) = @_; local($1);
  for ($attribute_name,@strings) {
    # just in case, handle non-octet characters:
    s/([^\000-\377])/sprintf('\\x{%04x}',ord($1))/eg and
      do_log(-1,"proto_encode: non-octet character encountered: %s", $_);
  }
  $attribute_name =~    # encode all but alfanumerics, . _ + -
    s/([^0-9a-zA-Z._+-])/sprintf("%%%02x",ord($1))/eg;
  for (@strings) {      # encode % and nonprintables
    s/([^\041-\044\046-\176])/sprintf("%%%02x",ord($1))/eg;
  }
  $attribute_name . '=' . join(' ',@strings);
}

sub proto_decode($) {
  my($str) = @_; local($1);
  $str =~ s/%([0-9a-fA-F]{2})/pack("C",hex($1))/egs;
  $str;
}

sub split_localpart($$) {
  my($localpart, $delimiter) = @_;
  my($owner_request_special) = 1;  # configurable ???
  my($extension); local($1,$2);
  if ($localpart =~ /^(postmaster|mailer-daemon|double-bounce)\z/i) {
    # do not split these, regardless of what the delimiter is
  } elsif ($delimiter eq '-' && $owner_request_special &&
           $localpart =~ /^owner-.|.-request\z/si) {
    # don't split owner-foo or foo-request
  } elsif ($localpart =~ /^(.+?)(\Q$delimiter\E.*)\z/s) {
    ($localpart, $extension) = ($1, $2);  # extension includes a delimiter
    # do not split the address if the result would have a null localpart
  }
  ($localpart, $extension);
}

sub unique_ref(@) {
  my($r) = @_ == 1 && ref($_[0]) ? $_[0] : \@_;  # accept list, or a list ref
  my(%seen);  my(@result) = grep { defined($_) && !$seen{$_}++ } @$r;
  \@result;
}

sub make_query_keys($$$;$) {
  my($addr,$at_with_user,$include_bare_user,$append_string) = @_;
  my($localpart,$domain) = split_address($addr); $domain = lc($domain);
  my($saved_full_localpart) = $localpart;
  $localpart = lc($localpart);  ### if !c('localpart_is_case_sensitive');
  # chop off leading @, and trailing dots
  local($1);
  $domain = $1  if $domain =~ /^\@?(.*?)\.*\z/s;
  my($extension); my($delim) = '+';  ### c('recipient_delimiter');
  if ($delim ne '') {
    ($localpart,$extension) = split_localpart($localpart,$delim);
    # extension includes a delimiter since amavisd-new-2.5.0!
  }
  $extension = ''  if !defined $extension;  # mute warnings
  my($append_to_user,$prepend_to_domain) = $at_with_user ? ('@','') : ('','@');
  my(@keys);  # a list of query keys
  push(@keys, $addr);                        # as is
  push(@keys, $localpart.$extension.'@'.$domain)
    if $extension ne '';                     # user+foo@example.com
  push(@keys, $localpart.'@'.$domain);       # user@example.com
  if ($include_bare_user) {  # typically enabled for local users only
    push(@keys, $localpart.$extension.$append_to_user)
      if $extension ne '';                   # user+foo(@)
    push(@keys, $localpart.$append_to_user); # user(@)
  }
  push(@keys, $prepend_to_domain.$domain);   # (@)sub.example.com
  if ($domain =~ /\[/) {     # don't split address literals
    push(@keys, $prepend_to_domain.'.');     # (@).
  } else {
    my(@dkeys); my($d) = $domain;
    for (;;) {               # (@).sub.example.com (@).example.com (@).com (@).
      push(@dkeys, $prepend_to_domain.'.'.$d);
      last  if $d eq '';
      $d = ($d =~ /^([^.]*)\.(.*)\z/s) ? $2 : '';
    }
    if (@dkeys > 10) { @dkeys = @dkeys[$#dkeys-9 .. $#dkeys] }  # sanity limit
    push(@keys,@dkeys);
  }
  if (defined $append_string && $append_string ne '') {
    $_ .= $append_string  for @keys;
  }
  my($keys_ref) = unique_ref(\@keys);  # remove duplicates
  ll(5) && do_log(5,"query_keys: %s", join(', ',@$keys_ref));
  # the rhs replacement strings are similar to what would be obtained
  # by lookup_re() given the following regular expression:
  # /^( ( ( [^\@]*? ) ( \Q$delim\E [^\@]* )? ) (?: \@ (.*) ) )$/xs
  my($rhs) = [   # a list of right-hand side replacement strings
    $addr,                  # $1 = User+Foo@Sub.Example.COM
    $saved_full_localpart,  # $2 = User+Foo
    $localpart,             # $3 = user
    $extension,             # $4 = +foo
    $domain,                # $5 = sub.example.com
  ];
  ($keys_ref, $rhs);
}

sub lookup_hash($$;$%) {
  my($addr, $hash_ref,$get_all,%options) = @_;
  ref($hash_ref) eq 'HASH'
    or die "lookup_hash: arg2 must be a hash ref: $hash_ref";
  local($1,$2,$3,$4); my(@matchingkey,@result); my($append_string);
  $append_string = $options{AppendStr}  if defined $options{AppendStr};
  my($keys_ref,$rhs_ref) = make_query_keys($addr,1,1,$append_string);
  for my $key (@$keys_ref) {   # do the search
    if (exists $$hash_ref{$key}) {  # got it
      push(@result,$$hash_ref{$key}); push(@matchingkey,$key);
      last  if !$get_all;
    }
  }
  # do the right-hand side replacements if any $n, ${n} or $(n) is specified
  for my $r (@result) {  # remember that $r is just an alias to array elements
    if (defined($r) && !ref($r) && index($r,'$') >= 0) { # plain string with $
      my($any) = $r =~ s{ \$ ( (\d+) | \{ (\d+) \} | \( (\d+) \) ) }
                        { my($j)=$2+$3+$4; $j<1 ? '' : $rhs_ref->[$j-1] }gxse;
      # bring taintedness of input to the result
      $r .= substr($addr,0,0)  if $any;
    }
  }
  if (!$get_all) { ($result[0], $matchingkey[0]) }
  else           { (\@result,   \@matchingkey)   }
}

sub lookup2($$$%) {
  my($get_all, $addr, $tables_ref, %options) = @_;
  (@_ - 3) % 2 == 0 or die "lookup2: options argument not in pairs (not hash)";
  my($label, @result,@matchingkey);
  for my $tb (!$tables_ref ? () : @$tables_ref) {
    my($t) = ref($tb) eq 'REF' ? $$tb : $tb; # allow one level of indirection
    if (!ref($t) || ref($t) eq 'SCALAR') {   # a scalar always matches
      my($r) = ref($t) ? $$t : $t;  # allow direct or indirect reference
      if (defined $r) {
        do_log(5,'lookup: (scalar) matches, result="%s"', $r);
        push(@result,$r); push(@matchingkey,"(constant:$r)");
      }
    } elsif (ref($t) eq 'HASH') {
      my($r,$mk);
      ($r,$mk) = lookup_hash($addr,$t,$get_all,%options)  if %$t;
      if (!defined $r)  {}
      elsif (!$get_all) { push(@result,$r);  push(@matchingkey,$mk)  }
      elsif (@$r)       { push(@result,@$r); push(@matchingkey,@$mk) }
    } else {
      die "TROUBLE: lookup table not implemented for object: " . ref($t);
    }
    last  if @result && !$get_all;
  }
  if (!$get_all) { ($result[0], $matchingkey[0]) }
  else           { (\@result,   \@matchingkey)   }
}

sub parse_quoted_rfc2821($$) {
  my($addr,$unquote) = @_;
  # the angle-bracket stripping is not really a duty of this subroutine,
  # as it should have been already done elsewhere, but we allow it here anyway:
  $addr =~ s/^\s*<//s;  $addr =~ s/>\s*\z//s;  # tolerate unmatched angle brkts
  local($1,$2); my($source_route,$localpart,$domain) = ('','','');
  # RFC 2821: so-called "source route" MUST BE accepted,
  #           SHOULD NOT be generated, and SHOULD be ignored.
  #           Path = "<" [ A-d-l ":" ] Mailbox ">"
  #           A-d-l = At-domain *( "," A-d-l )
  #           At-domain = "@" domain
  if (index($addr,':') >= 0 &&  # triage before more testing for source route
      $addr =~ m{^ (       [ \t]* \@ (?: [0-9A-Za-z.!\#\$%&*/^{}=_+-]* |
                                   \[ (?: \\. | [^\]\\] ){0,999} \] ) [ \t]*
                     (?: , [ \t]* \@ (?: [0-9A-Za-z.!\#\$%&*/^{}=_+-]* |
                                   \[ (?: \\. | [^\]\\] ){0,999} \] ) [ \t]* )*
                     : [ \t]* ) (.*) \z }xs)
  { # NOTE: we are quite liberal on allowing whitespace around , and : here,
    # and liberal in allowed character set and syntax of domain names,
    # we mainly avoid stop-characters in the domain names of source route
    $source_route = $1; $addr = $2;
  }
  if ($addr =~ m{^ ( .*? )
                 ( \@ (?: [^\@\[\]]+ | \[ (?: \\. | [^\]\\] ){0,999} \]
                          | [^\@] )* )
                 \z}xs) {
    ($localpart,$domain) = ($1,$2);
  } else {
    ($localpart,$domain) = ($addr,'');
  }
  $localpart =~ s/ " | \\ (.) | \\ \z /$1/xsg  if $unquote; # undo quoted-pairs
  ($source_route, $localpart, $domain);
}

sub unquote_rfc2821_local($) {
  my($mailbox) = @_;
  my($source_route,$localpart,$domain) = parse_quoted_rfc2821($mailbox,1);
  # make address with '@' in the localpart but no domain (like <"aa@bb.com"> )
  # distinguishable from <aa@bb.com> by representing it as aa@bb.com@ in
  # unquoted form; (it still obeys all regular rules, it is not a dirty trick)
  $domain = '@'  if $domain eq '' && $localpart ne '' && $localpart =~ /\@/;
  $localpart . $domain;
}

#
# ======================================================================
# Code above is copied from amavisd; some day it should be factored out.
# Code from here on is specific to amavisd-signer.
# ======================================================================
#

# process a request to choose a signing key;
#
sub choose_key_request($) {
  my($attr) = @_;
  my(@results);
  my(%sig_options);  # signature options, and constraints for choosing a key
  my(%key_options);  # options associated with a signing key
  my(@tried_domains);  # used for logging a failure
  my($chosen_addr,$chosen_addr_src);
  my($cand) = $attr->{candidate};
  my(@candidates) = !defined $cand ? () : !ref $cand ? $cand : @$cand;
  my($sobm) = \@dkim_signature_options_bysender_maps;
  for my $pair (@candidates) {
    my($addr_src,$addr) = split(' ',$pair,2);
    $addr = unquote_rfc2821_local($addr);
    my($addr_localpart,$addr_domain) = split_address($addr);
    $addr_domain = lc($addr_domain);
    my($dkim_options_ref,$mk_ref) = lookup2(1,$addr,$sobm);
    $dkim_options_ref = []  if !defined $dkim_options_ref;  #***?
    # place catchall default(s) at the end of the list of options;
    push(@$dkim_options_ref, { c => 'relaxed/simple', a => 'rsa-sha256' });
    %sig_options = ();  # signature options:
                  # (v), a, (b), (bh), c, d, (h), i, (l), q, s, (t), x, (z)
    # traverse from specific to general, first match wins
    for my $opts_hash_ref (@$dkim_options_ref) {
      while (my($k,$v) = each(%$opts_hash_ref))
        { $sig_options{$k} = $v  if !exists($sig_options{$k}) }
    }
    # a default for a signing domain is a domain of each tried address
    if (!exists($sig_options{d}))
      { my($d) = $addr_domain; $d =~ s/^\@//; $sig_options{d} = $d }
    push(@tried_domains, $sig_options{d});
    ll(5) && do_log(5, "signature options for %s(%s): %s", $addr,$addr_src,
            join('; ', map { $_.'='.$sig_options{$_} } keys %sig_options));
    # find a private key associated with a signing domain and selector,
    # and meeting constraints
    %key_options = get_dkim_key(%sig_options)
      if defined $sig_options{d} && $sig_options{d} ne '';
    my($key) = $key_options{key};
    if (defined $key && $key ne '') {  # found; copy the key and its options
      $sig_options{key} = $key;  $sig_options{s} = $key_options{selector};
      $chosen_addr = $addr; $chosen_addr_src = $addr_src;
      last;
    }
  }
  # if any signature options were specified in the request and not overruled
  # by more specific ones here, copy them to the resulting set of sig options
  for my $opt (keys %$attr) {
    if ($opt =~ /^sig\.(.+)\z/) {
      $sig_options{$1} = $attr->{$opt}  if !exists($sig_options{$1});
    }
  }
  ll(5) && do_log(5, "sig options: %s",
             join('; ', map { $_.'='.$sig_options{$_} } keys %sig_options));
  my(%key_options);
  if (defined $sig_options{d} && $sig_options{d} ne '') {
    %key_options = get_dkim_key(%sig_options);
  }
  do_log(5, "key options: %s is %s",
            $_, $key_options{$_}) for keys %key_options;
  my($s) = $key_options{'selector'};
  my($d) = $key_options{'domain'};
  $sig_options{'s'} = $s;
  $sig_options{'d'} = $d;
  delete $sig_options{'key'};  # no use of key ref in the protocol
  for my $opt (sort keys %sig_options) {
    if (defined $sig_options{$opt}) {
      push(@results, proto_encode('sig.'.$opt, $sig_options{$opt}));
    }
  }
  # optional information if available: client may log it, or use for debugging
  if (defined $chosen_addr_src && defined $chosen_addr) {
    push(@results, proto_encode('chosen_candidate',
                                $chosen_addr_src, $chosen_addr));
  }
  \@results;
}

# sign a digest code using the specified algorithm and a private signing key
#
sub dkim_rsa_sign($$$) {
  my($digest,$alg_name,$key) = @_;
  my($result);
  $digest = ''   if !defined $digest;
  $alg_name = '' if !defined $alg_name;
  if (defined $key && $key ne '') {
    my($key) = Mail::DKIM::PrivateKey->load(Cork => $key);
    $key  or die "no key available\n";
    $result = $key->sign_digest($alg_name,$digest);
  }
  $result;
}

# process a request to sign the supplied digest with a selected key
#
# presence of the 'b' attribute in the result indicates success,
# otherwise the result is treated as signature unavailable
#
sub sign_request($) {
  my($attr) = @_;
  my(@results, $reason, $sig);
  my($digest, $digest_alg, $selector, $domain) =
    @$attr{qw(digest digest_alg s d)};
  if (!defined $digest || $digest eq '') {
    $reason = 'cannot sign, digest not provided, nothing to sign';
  } elsif (!defined $digest_alg || $digest_alg eq '') {
    $reason = 'cannot sign, digest algorithm name not provided';
  } elsif (!defined $domain || $domain eq '') {
    $reason = 'cannot sign, signing domain not provided';
  } elsif (!defined $selector || $selector eq '') {
    $reason = 'cannot sign, selector not provided';
  } else {
    my(%sig_options);  # signature options: v, a, c, d, h, i, l, q, s, t, x, z
    $sig_options{s} = $selector;
    $sig_options{d} = $domain;
    my(%key_options) = get_dkim_key(%sig_options);
    if (!defined $key_options{key}) {
      $reason = 'cannot sign, signing key not available';
    } else {
      do_log(5, "key options: %s is %s",
                $_, $key_options{$_})  for keys %key_options;
      eval {
        $sig = dkim_rsa_sign(decode_base64($digest),
                             $digest_alg, $key_options{key});  1;
      } or do {
        my($eval_stat) = $@ ne '' ? $@ : "errno=$!";  chomp $eval_stat;
        do_log(0, "signing failed: %s", $eval_stat);
        $reason = 'cannot sign: ' . $eval_stat;
      };
      push(@results, proto_encode('d', $key_options{'domain'}));
      push(@results, proto_encode('s', $key_options{'selector'}));
    }
  }
  if (defined $sig && $sig ne '') {
    push(@results, proto_encode('b', encode_base64($sig,'')));
  } else {
    $reason = 'cannot sign: signing failed'  if !defined $reason;
    push(@results, proto_encode('reason', $reason));
  }
  \@results;
}

# process the request received from amavisd
#
sub do_the_request($) {
  my($attr) = @_;
  ll(2) && do_log(2, "got: %s", join('; ', map {
                      my($k) = $_; my($v) = $attr->{$k};
                      map { $k.'='.$_ } (!ref $v ? $v : @$v) } keys %$attr));
  my(@results);
  my($req_id) = $attr->{request_id};
  my($log_id) = $attr->{log_id};
  push(@results, proto_encode('request_id', $req_id))  if defined $req_id;
  push(@results, proto_encode('log_id',     $log_id))  if defined $log_id;
  my($request_type) = $attr->{request};
  $request_type = ''  if !defined $request_type;
  if ($request_type eq 'choose_key') {
    push(@results, @{choose_key_request($attr)});
  } elsif ($request_type eq 'sign') {
    push(@results, @{sign_request($attr)});
  } else {
    push(@results, proto_encode('reason', 'unknown request type'));
    do_log(2, "got: ignoring request: %s", $request_type);
  }
  ll(1) && do_log(1, "response: %s", join('; ', @results));
  do_log(5, "");
  \@results;
}

# IO::Multiplex -style callback hook
#
sub mux_connection {
  my($self,$mux,$fh) = @_;
  do_log(3, "client %s just connected", $self->{peeraddr});
  $self->{attr} = {};
}

# the mux_connection callback is guaranteed to have already been run once
#
sub mux_input {
  my($self,$mux,$fh,$in_ref) = @_;
  my $attr = $self->{attr};
  do_log(5, "input from %s ready", $self->{peeraddr});

  # process each line in the input, leaving partial lines in the input buffer
  local($1,$2); my($quit) = 0;
  while ($$in_ref =~ s/^(.*?)\015?\012//) {
    my($ln) = $1;
    if ($ln eq '') {  # empty line indicates end of a request
      my($results_ref) = do_the_request($attr);
      print(join('', map { $_."\015\012" } (@$results_ref,'')))
        or do_log(0,"mux_input: error writing a response to socket" );
      %$attr = ();  # reset, awaiting next request in the same session
    } elsif ($ln =~ /^ ([^=\000\012]*?) (?: = | : [ \t]* ) (.*) \z/xsi) {
      my($attr_name) = proto_decode($1);
      my($attr_val)  = proto_decode($2);
      if (!exists $attr->{$attr_name}) {
        $attr->{$attr_name} = $attr_val;  # simple scalar for one-time attrs
      } elsif (!ref($attr->{$attr_name})) {  # multiple, convert to a list
        $attr->{$attr_name} = [ $attr->{$attr_name}, $attr_val ];
      } else {  # append to a list of same-name attributes
        push(@{$attr->{$attr_name}}, $attr_val);
      }
    } else {
      do_log(0, "mux_input: ignored line: %s", $ln);
    }
  }
  close(STDOUT)  if $quit;
}


#
# Main program starts here (after initializations near the top of this file)
#


dkim_key_postprocess();

# set up a Net::Server configuration
$server = AmavisSigner->new({
  # limit socket bind (e.g. to the loopback interface)
  host => (!defined $inet_socket_bind || $inet_socket_bind eq '' ? '*'
                                                        : $inet_socket_bind),
  port => \@listen_sockets,  # listen on these sockets (Unix or inet)
  listen => $listen_queue_size,  # undef for a default
  user  => ($> == 0 || $< == 0) ? $daemon_user  : undef,
  group => ($> == 0 || $< == 0) ? $daemon_group : undef,
  background => $daemonize ? 1 : undef,
  setsid     => $daemonize ? 1 : undef,
  chroot     => $daemon_chroot_dir ne '' ? $daemon_chroot_dir : undef,
  pid_file   => $pid_file,
  log_file   => $daemonize ? 'Sys::Syslog' : undef,
  syslog_ident    => $syslog_ident,
  syslog_facility => $syslog_facility,
  syslog_logsock  => 'native',
  # 0=err, 1=warning, 2=notice, 3=info, 4=debug
  log_level => $log_level >= 5 ? 4 : 2,
});

$server->run;  # transferring control to Net::Server
exit 1;  # shouldn't get here

# TODO: pkcs11 URI
# In order to use a key an application needs the path to the PKCS11 lib,
# the key ID, username, pin and the slot number
#
# http://blogs.sun.com/janp/entry/pkcs_11_engine_patch_including
#   pkcs11:[object=<label>]  # object (key) label, eg. "mykey"
#   [;token=<label>]         # token label
#   [;manuf=<label>]         # manufacturer ID
#   [;serial=<label>]        # serial number of the token
#   [;model=<label>]         # token model
#   [;objecttype=(public|private|cert|data)]
#   [;passphrasedialog=(builtin|exec:<file>)]
#
# alternative:
#   pkcs11:///path/to/pkcs11/lib?slot=0&id=123
#   file:///path/to/pem/file
#
# SEE: http://blog.nominet.org.uk/tech/category/crypto/
