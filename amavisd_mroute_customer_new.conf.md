#!/usr/local/bin/perl - for vim syntax coloring

# 2018-04-25 - tj - initial release to production
# 2022-03-09 - tj - add new wblist features:
#    wblist by sender (like always)
#    wblist by rcfd
#    wblist by dkim
#    wblist by ?

# TODO
#	  Fix RCVD check
#   should the wblist table include a priority? 
#   Should there be multiple "extra_checks" in a single record, or store in separate records and loop over them?
#   Test all possible combinations - make sure that things override in a proper fashion
#   Add really understandable headers explaining each wblist, etc.
#   Add check for a keyword in the body?
#   Quarantine to IMAP server - add a header "X-MailRoute-Qtn: <category>" and have a sieve on the server put it in the right folder?
#
#   Maybe: expiration date for wblist entry?
#   Maybe: tag and deliver bulk or transactional email?
#   Maybe: paubox-type feature.  If sending to non-tls, send a message instead
#      with a secure link to the message stored in our database.

package Amavis::Custom;
use strict;
use Data::Dumper;

my $default_unwanted_language_score = 5;

my $ll = 2;  #log level 

my $outbound_user_query = "SELECT e.uuid AS id, p.sa_userconf, 7 as priority, 'user' AS label
    FROM amavisd_emailaccount AS e
      INNER JOIN amavisd_localpartalias AS la
        ON (e.id=la.email_account_id)
      INNER JOIN amavisd_policy AS p
        ON (e.policy_id=p.id)
      INNER JOIN amavisd_domainalias AS da
        ON (la.domain_id=da.domain_id)
    WHERE da.name  = ? AND la.localpart = ?
UNION
SELECT e.uuid AS id, p.sa_userconf, 6 as priority, 'extalias' AS label
    FROM amavisd_emailaccount AS e
     INNER JOIN amavisd_localpartalias AS la
       ON (e.id=la.email_account_id)
    INNER JOIN amavisd_policy AS p
       ON (e.policy_id=p.id)
   WHERE la.external_domain = ? AND la.localpart = ? 
UNION
SELECT d.uuid AS id, p.sa_userconf, 5 as priority, 'domain' AS label
    FROM amavisd_domainalias AS da
      INNER JOIN amavisd_domain AS d
        ON (da.domain_id=d.id)
      INNER JOIN amavisd_policy AS p
        ON (d.policy_id=p.id)
      INNER JOIN amavisd_customer AS c
        ON (d.customer_id=c.id)
    WHERE da.name = ? 
UNION
SELECT '00000000-0000-0000-0000-000000000000' AS id, p.sa_userconf, 1 as priority, 'default' AS label
    FROM amavisd_policy AS p
    WHERE id = 28301
ORDER BY priority DESC;
";

my $sql_wblist_new_query = "SELECT wb,extra_checks FROM tj_wblist w INNER JOIN amavisd_mailaddr a ON w.sid=a.id WHERE w.rid=? AND a.email IN (%s) ORDER BY a.priority DESC, length(a.email) DESC LIMIT 1";


#wblist table:
#
#CREATE TABLE `tj_wblist` (
#  `id` int(11) NOT NULL AUTO_INCREMENT,
#  `sid` int(11) NOT NULL,
#  `rid` varchar(36) DEFAULT NULL,
#  `wb` varchar(10) NOT NULL,
#  `extra_checks` varchar(1024) DEFAULT NULL,
#  PRIMARY KEY (`id`),
#  UNIQUE KEY `sid` (`sid`,`rid`),
#  KEY `rid` (`rid`)
#) ENGINE=InnoDB DEFAULT CHARSET=utf8;
# extra_checks is a string, delimited by tabs.  values:
#  dkim <- match if dkim matches
#  rcvd=ip,ip,server.domain.com
#  
#  terramar.net 9a62a13e-0ea3-11e3-ae18-002590a497ef did
#  tj@terramar.net 090ec5d6-0ea4-11e3-ae18-002590a497ef uid
#  
#  mailroute.net 67621
#  tj@mailroute.net  57817
#  INSERT into tj_wblist values (NULL,67621,'090ec5d6-0ea4-11e3-ae18-002590a497ef','B',NULL);
#
#


BEGIN {
    import Amavis::Lookup; #qq(lookup lookup2);
    import Amavis::Util qw(min max minmax untaint untaint_inplace ll do_log unique_list
                  format_time_interval get_deadline
                  idn_to_ascii mail_addr_idn_to_ascii idn_to_utf8
                  safe_encode_utf8 proto_encode proto_decode);
    import Amavis::rfc2821_2822_Tools qw(make_query_keys split_address 
                  quote_rfc2821_local qquote_rfc2821_local);
    import Amavis::Conf qw(:platform :legacy_confvars :confvars 
                  :dynamic_confvars :legacy_dynamic_confvars :confvars 
                  :sa c cr ca @lookup_sql_dsn);
    import Amavis::Timing qw(section_time);
}

sub check_in_msginfo_rcvd ($$) {
  my ($msginfo, $value) = @_;
  my $matchdomain = lc $value;
  do_log($ll,"CUSTOM: whitelist_from_rcvd (checking for $matchdomain)");
print "a\n";
  if (!$matchdomain) {
    return 0;
  }
print "b\n";

  my $trace = $msginfo->trace;
  my $match;
 
TODO: THIS ISN'T WORKIGN 
  foreach my $t (@$trace) {
      my $hostname = lc $t->{from};
      my $hostip = $t->{ip};
print "matching '$matchdomain' with $hostname, $hostip\n";
      if ($matchdomain =~ m{^ \[ (.*) \] ( / \d{1,3} )? \z}sx) {  # matching by IP address
          my($wl_ip, $rly_ip) = ($1, $hostip);
          $wl_ip .= $2 if defined $2; # allow prefix len even after bracket
#print "wl_ip: $wl_ip, rly_ip: $rly_ip\n";
          if (!defined $rly_ip || $rly_ip eq '') {
              # relay's IP address not provided or unparseable
          } elsif ($wl_ip =~ /^\d+\.\d+\.\d+\.\d+\z/s) {
              if ($wl_ip eq $rly_ip) { $match = 1; last }  # exact match
          } elsif ($wl_ip =~ /^[\d\.]+\z/s) {  # an IPv4 classful subnet?
              $wl_ip =~ s/\.*\z/./;  # enforce trailing dot
              if ($rly_ip =~ /^\Q$wl_ip\E/) { $match = 1; last }  # subnet
          } else {  # either an wl entry is an IPv6 addr, or has a prefix len
              my $rly_ip_obj = NetAddr::IP->new($rly_ip);  # TCP-info field
              if (!defined $rly_ip_obj) {
                  do_log($ll, "rules: bad IP address in relay: " . $rly_ip);
              } else {
                  my $wl_ip_obj = NetAddr::IP->new($wl_ip); # whitelist 2nd param
                  if (!defined $wl_ip_obj) {
                      do_log($ll, "rules: bad IP address in whitelist: " . $wl_ip);
                  } elsif ($wl_ip_obj->contains($rly_ip_obj)) {
                      # note: an IPv4-compatible IPv6 address can match an IPv4 addr
                      do_log($ll, "rules: relay addr " . $rly_ip . " matches whitelist " . $wl_ip_obj );
                      $match = 1; last;
                  } else {
                      do_log($ll, "rules: relay addr " . $rly_ip . " does not match wl " . $wl_ip_obj );
                  }
              }
          }
          
      } else {  # match by a rdns name
          my $rdns = $hostname;
          if ($rdns =~ /(?:^|\.)\Q${matchdomain}\E$/i) { $match=1; last }
      }
  }
  return $match;
}

sub white_black_list($$$$$$) {
  my($self,$msginfo,$sql_wblist,$user_id_sql,$ldap_lookups) = @_;
  # "$self" is the Amavis::Custom object - so we can reuse conn_h, etc..
  my $fm = $msginfo->rfc2822_from;
  my(@rfc2822_from) = !defined($fm) ? () : ref $fm ? @$fm : $fm;
  my(@senders) = ($msginfo->sender, @rfc2822_from);
  @senders = unique_list(\@senders);  # remove possible duplicates
  ll(4) && do_log(4,"wbl: checking sender %s",
                    scalar(qquote_rfc2821_local(@senders)));
  my($any_w,$any_b,$all,$wr,$br);
  $any_w = 0; $any_b = 0; $all = 1;

  my($conn_h) = $self->{'conn_h'};
  $conn_h->begin_work_nontransaction;  # (re)connect if not connected


  for my $r (@{$msginfo->per_recip_data}) {  # for each recipient
    next  if $r->recip_done;  # already dealt with
    my($wb,$boost); my $found = 0; my $recip = $r->recip_addr;
    my($user_id_ref,$mk_ref);
    $user_id_ref = $r->user_id;
    $user_id_ref = []  if !defined $user_id_ref;
    do_log(5,"wbl: (SQL) recip <%s>, %s matches",
             $recip, scalar(@$user_id_ref))  if $sql_wblist && ll(5);
    for my $sender (@senders) {
      for my $ind (0..$#{$user_id_ref}) { # for ALL SQL sets matching the recip
        my $user_id = $user_id_ref->[$ind];  my $mkey;
        #($wb,$mkey) = lookup(0,$sender,
        #        Amavis::Lookup::SQLfield->new($sql_wblist,'wb','S',$user_id) );
        my($keys_ref,$rhs_ref) = make_query_keys($sender,0,1);
        if (!$sql_allow_8bit_address) { s/[^\040-\176]/?/gs for @$keys_ref }
        my $n = scalar(@$keys_ref);  # number of keys
        #join(',', ('?') x $n)
        my $wblist_query = sprintf($sql_wblist_new_query, join(',', ('?') x $n));
        my @query_ops;
        push @query_ops, $user_id;
        push @query_ops, @$keys_ref;
   	for (@query_ops) {
    		if (ref $_) { untaint_inplace($_->[0]) } else { untaint_inplace($_) }
  	}
        $conn_h->execute($wblist_query, @query_ops);  # do the query
        my $a_ref;
	my $wb; my $extra_checks;
        while ( defined($a_ref=$conn_h->fetchrow_arrayref($wblist_query)) ) {
	    $wb = $a_ref->[0]; 
	    $extra_checks = $a_ref->[1]; 
        }

print "\n\ntj------------------------------\n\n";
print "sender: $sender\n";
print "wb: $wb\n";
print "extra_check: $extra_checks\n";
print "\n\ntj------------------------------\n\n";
        $conn_h->finish($wblist_query);


        do_log(4,'wbl: (SQL) recip <%s>, rid=%s, got: "%s" "%s"',
                 $recip,$user_id,$wb, $extra_checks);
	
print "\n\ntj------------------------------\n\n";
        for my $e (split(/\t/,$extra_checks)) { 	
          if ($e =~ /^dkim/) {
            my($sigs_ref) = $msginfo->dkim_signatures_valid;
            do_log($ll,"CUSTOM: dkim valid, d=%s", join(',', map {$_->domain} @$sigs_ref));
            if (defined $sigs_ref && @$sigs_ref) {
              do_log($ll,"CUSTOM: wblist dkim check passes");
              # has valid dkim signature
            } else {
              $wb = undef;
            }
          } elsif ($e =~ /^rcvd/) {
            my ($a, $b) = split (/=/, $e);
            my $rcvd_match = check_in_msginfo_rcvd($msginfo, $b);
print "*****rcvd_match: $rcvd_match\n";
	    if ($rcvd_match) {
              do_log($ll,"CUSTOM: wblist rcvd check passes '$b'");
            } else {
               $wb = undef;
            }
	  }
        }
print "***********WB: $wb\n";
	# we have a match, so adjust score or white/blacklist
        if (!defined($wb)) {
          # NULL field or no match: remains undefined
        } elsif ($wb =~ /^ *([+-]?\d+(?:\.\d*)?) *\z/) {  # numeric
          my $val = 0+$1;  # penalty points to be added to the score
          $boost += $val;
          ll(2) && do_log(2,
                  'wbl: (SQL) soft-%slisted (%s) sender <%s> => <%s> (rid=%s)',
                  ($val<0?'white':'black'), $val, $sender, $recip, $user_id);
          $wb = undef;  # not hard- white or blacklisting, does not exit loop
        } elsif ($wb =~ /^[ \000]*\z/) {        # neutral, stops the search
          $found=1; $wb = 0;
          do_log(5, 'wbl: (SQL) recip <%s> is neutral to sender <%s>',
                    $recip,$sender);
        } elsif ($wb =~ /^([BbNnFf])[ ]*\z/) {  # blacklisted (B,N(o), F(alse))
          $found=1; $wb = -1; $any_b++; $br = $recip;
          $r->recip_blacklisted_sender(1);
          do_log(5, 'wbl: (SQL) recip <%s> blacklisted sender <%s>',
                    $recip,$sender);
        } else {            # whitelisted (W, Y(es), T(true), or anything else)
          if ($wb =~ /^([WwYyTt])[ ]*\z/) {
            do_log(5, 'wbl: (SQL) recip <%s> whitelisted sender <%s>',
                      $recip,$sender);
          } else {
            do_log(-1,'wbl: (SQL) recip <%s> whitelisted sender <%s>, '.
                      'unexpected wb field value: "%s"', $recip,$sender,$wb);
          }
          $found=1; $wb = +1; $any_w++; $wr = $recip;
          $r->recip_whitelisted_sender(1);
        }
        last  if $found;
      }

    } # endfor on @senders
    if ($boost) {  # defined and nonzero
      $r->spam_level( ($r->spam_level || 0) + $boost);
      my $spam_tests = 'AM.WBL=' . (0+sprintf("%.3f",$boost));
      if (!$r->spam_tests) {
        $r->spam_tests([ \$spam_tests ]);
      } else {
        unshift(@{$r->spam_tests}, \$spam_tests);
      }
    }
    $all = 0  if !$wb;
  } # endfor on recips
  if (!ll(2)) {
    # don't bother preparing a log report which will not be printed
  } else {
    my $msg = '';
    if    ($all && $any_w && !$any_b) { $msg = "whitelisted" }
    elsif ($all && $any_b && !$any_w) { $msg = "blacklisted" }
    elsif ($all) { $msg = "black or whitelisted by all recips" }
    elsif ($any_b || $any_w) {
      $msg .= "whitelisted by ".($any_w>1?"$any_w recips, ":"$wr, ") if $any_w;
      $msg .= "blacklisted by ".($any_b>1?"$any_b recips, ":"$br, ") if $any_b;
      $msg .= "but not by all,";
    }
    do_log(2,"wbl: %s sender %s",
             $msg, scalar(qquote_rfc2821_local(@senders)))  if $msg ne '';
  }
  ($any_w+$any_b, $all);
}




sub new { 
	my($class,$conn,$msginfo) = @_; 
	my($self) = bless {}, $class; 
 
    # open up a mysql connection for this custom hook to use. 
    my($conn_h) = Amavis::Out::SQL::Connection->new(@lookup_sql_dsn);
    $self->{'conn_h'} = $conn_h;

	$self; # returning an object activates further callbacks, 
		# returning undef disables them 
} 


sub checks {  
  my($self,$conn,$msginfo) = @_;

  do_log($ll,"CUSTOM Hook: checks");

  my @hdrinfo;
  
  my $uconf_maps_ref = ca('sa_userconf_maps');
  for my $r (@{$msginfo->per_recip_data}) {

    next  if $r->recip_done;  # already dealt with

    my($is_local) = $r->recip_is_local; # recipient matches @local_domains_maps

    my $user_id_ref;

    my $uconf;
    if (!$is_local) { # outgoing mail 
        # for outbound mail, we have to find the settings for our user manually - amavisd hasn't done a policy
        # lookup for the sender, or anything like that.  We need to get the sa_userconf setting, and the uuids
        # for the user, alias, domain, default
        my $sender = $msginfo->sender; 
        my ($localpart, $domain) = split(/@/, $sender); 

        my($conn_h) = $self->{'conn_h'};
        $conn_h->begin_work_nontransaction;  # (re)connect if not connected
        
        my @query_ops;
        push @query_ops, untaint $domain; 
        push @query_ops, untaint $localpart;
        push @query_ops, untaint $domain; 
        push @query_ops, untaint $localpart;
        push @query_ops, untaint $domain;
        $conn_h->execute($outbound_user_query, @query_ops);  # do the query

        # build the uuid array, and check to see if any of them have a "custom" setting in sa_userconf
        my @user_id_arr;
        my $a_ref;
        while ( defined($a_ref=$conn_h->fetchrow_arrayref($outbound_user_query)) ) {
            $uconf='custom' if ($a_ref->[1])  eq 'custom';
            push (@user_id_arr, $a_ref->[0]);
        }

        $conn_h->finish($outbound_user_query)  if defined $a_ref;  # only if not all read

        $user_id_ref = \@user_id_arr;

    } else  {
        # this is all much easier with inbound mail - it's all been done for us already
        my $r_addr = $r->recip_addr;
        $uconf = Amavis::Lookup::lookup2(0, $r_addr, $uconf_maps_ref)  if @$uconf_maps_ref;
        $user_id_ref = $r->user_id;
    }

    $uconf = ''  if !defined $uconf;
    if ($uconf =~ /^custom/i) {
        do_log($ll,"CUSTOM: user has custom spamassassin overrides");

        # let's get some info for later use, clean it up, etc...
        my($m_id) = $msginfo->get_header_field_body('message-id');  # e.g. <12@e.n>
        my($subj) = $msginfo->get_header_field_body('subject');
        my($from) = $msginfo->get_header_field_body('from');
        my($sender) = $msginfo->sender;
        # e.g.: "=?ISO-8859-1?Q?Ren=E9_van_den_Berg?=" <vd@example.com>
        for ($m_id,$from,$subj) {  # RFC2047-decode char. sets in some header fields
            local($1); chomp; my($str);
            s/\n([ \t])/$1/sg; s/^[ \t]+//s; s/[ \t]+\z//s;  # unfold, trim
            eval { $str = safe_decode('MIME-Header',$_) };   # to string of logical chr
            $_ = $str  if $@ eq '';  # replace if all ok, otherwise keep unchanged
        }
    
        # @tests, %scores are used if a rule wants to add/delete tests, or change their scores.
        #  They are used to readjust the score and/the lists of tests that hit at the end of processing the msg
        #my @tests = split(/,/, $msginfo->supplementary_info('TESTS'));
#print Dumper $msginfo;
    	my $spam_tests = $r->spam_tests;
#print Dumper $spam_tests;
        my @testsscores = split(/,/, join(',',map($$_,@$spam_tests))) if $spam_tests;
#print Dumper @testsscores;
        my %scores;
        for my $s (@testsscores) {
            my ($rule, $sc) = split(/=/, $s);
            $scores{$rule} = $sc;
        }
        # look up our custom amavisd_sauserprefsettings 
        $user_id_ref = []  if !defined $user_id_ref;
        my $userpref_query = "select * from amavisd_sauserpref where uuid in (" . join(', ', ('?') x @$user_id_ref)  . ") order by priority DESC";
        my($conn_h) = $self->{'conn_h'};
        $conn_h->begin_work_nontransaction;  # (re)connect if not connected

        my @query_ops;
        foreach my $a (@$user_id_ref) {
            push @query_ops, untaint($a);
        }
        $conn_h->execute($userpref_query,@query_ops);  # do the query
        
       
        # going to just loop through and check each one separately.  Could make this more efficient if
        # we were to loop through all the settings, build up tables of each thing to check, and then checking
        # them all at once.  For example, build up a list of pairs of senders and domains 
        # for whitelist_from_rcvd, and then check them each against all the received headers at once, instead
        # of doing them over and over.  But I don't think that this is much of a performance hit.

        my($a_ref); 

        my @score_rules;
 
        while ( defined($a_ref=$conn_h->fetchrow_arrayref($userpref_query)) ) {
            my $preference = $a_ref->[4];
            my $value = $a_ref->[5];
            my $direction = $a_ref->[7] ;  # this will be "I"nbound, "O"utbound, "B"oth

            # Check to see if this rule applies to this email - inbound/outbound/both
            next unless ( ($is_local && $direction eq "I") || (!$is_local && $direction eq "O") || ($direction eq "B")  );
            
            # loop through each custom setting and do that particular check
            #

            for ($preference) {
                if (/^score/) { 
                    # process scores last, in case an earlier rule made a change that could be affected by a score rule too
                    push @score_rules, [ $preference, $value ];
                }
                elsif (/^ok_languages/) { 

                    do_log($ll,"CUSTOM: ok_languages($value)");
#                   # if user has their own ok_languages settings, we have to override the SA one
                    delete $scores{'UNWANTED_LANGUAGE_BODY'} if ( $scores{'UNWANTED_LANGUAGE_BODY'} );

                    my @languages = split(' ', lc $value);
                    if (grep {$_ eq "all" } @languages) {
                        next;
                    }
                    my @matches = split (' ', $msginfo->supplementary_info('LANGUAGES'));
                    next if ! @matches;
       
                    my $found = 0; 
                    foreach my $match (@matches) {
                        $match =~ s/\..*//;
                        foreach my $language (@languages) {
                            if ($match eq $language) {
                                #print "found $match in ok_languages\n";
                                $found++;
                            } 
                        }
                    }
                    # language in list not found, so add this rule to the scores hash
                    if (!$found) { 
			my $matchstr = "Unwanted Lang: " . join '|', @matches;
			push @hdrinfo , $matchstr;
                        $scores{'UNWANTED_LANGUAGE_BODY'} = $default_unwanted_language_score;
                        do_log($ll,"CUSTOM: ok_language - language not in list, adding rule");
                     };
                }
#TODO TESTME
#                elsif (/^whitelist_from_auth/) { 
#                    my $matchvalue = lc $value;
#                    do_log($ll,"CUSTOM: whitelist_from_auth ($matchvalue)");
#                    next unless (defined $matchvalue  && $matchvalue !~ /^$/);
#                    # addr can be basic regexp, matching * or .
#                    my $re = $matchvalue;  # do I need to sanitize???
#                    $re =~ tr/?/./;                             # "?" -> "."
#                    $re =~ s/\*+/\.\*/g;                        # "*" -> "any string"
#                    if ( $from =~ qr/$re/i || $sender =~ qr/$re/i )  {
#                        do_log($ll, "from: $from, sender; $sender, re: $re");
#			if ( $scores{'SPF_PASS'} || $scores{'DKIM_VALID'} || $scores{'DKIM_VALID_AU'} ) {
#				$r->recip_whitelisted_sender(1);
#			    do_log($ll,"CUSTOM: WHITELISTED whitelist_from_auth '$matchvalue'");
#			    push @hdrinfo , "whitelist_auth: '$matchvalue'";
#                        }
#                    }
#                }
#
#
#
#TODO TESTME
#                elsif ( (/^whitelist_from_rcvd/) || (/^blacklist_from_rcvd/) ) { 
#                    my $wl = (/^whitelist_from_rcvd/) ? 1 : 0;
#                    my $matchvalue = lc $value;
#                    do_log($ll,"CUSTOM: whitelist_from_rcvd ($matchvalue)");
#                    next unless (defined $matchvalue  && $matchvalue !~ /^$/ && $matchvalue =~ /^\S+\s+\S+$/);
#                    my ($matchaddr, $matchdomain) = split(/\s+/, $matchvalue);  
#                    # addr can be basic regexp, matching * or .
#                    my $re = $matchaddr;  # do I need to sanitize???
#                    $re =~ tr/?/./;                             # "?" -> "."
#                    $re =~ s/\*+/\.\*/g;                        # "*" -> "any string"
#                    if ($from =~ qr/$re/i || $sender =~ qr/$re/i) {
#                        my $trace = $msginfo->trace;
#                        my $match;
#                        foreach my $t (@$trace) {
#                            my $hostname = lc $t->{from};
#                            my $hostip = $t->{ip};
#
#                            if ($matchdomain =~ m{^ \[ (.*) \] ( / \d{1,3} )? \z}sx) {  # matching by IP address
#                                my($wl_ip, $rly_ip) = ($1, $hostip);
#                                $wl_ip .= $2 if defined $2; # allow prefix len even after bracket
##print "wl_ip: $wl_ip, rly_ip: $rly_ip\n";
#                                if (!defined $rly_ip || $rly_ip eq '') {
#                                    # relay's IP address not provided or unparseable
#                                } elsif ($wl_ip =~ /^\d+\.\d+\.\d+\.\d+\z/s) {
#                                    if ($wl_ip eq $rly_ip) { $match = 1; last }  # exact match
#                                } elsif ($wl_ip =~ /^[\d\.]+\z/s) {  # an IPv4 classful subnet?
#                                    $wl_ip =~ s/\.*\z/./;  # enforce trailing dot
#                                    if ($rly_ip =~ /^\Q$wl_ip\E/) { $match = 1; last }  # subnet
#                                } else {  # either an wl entry is an IPv6 addr, or has a prefix len
#                                    my $rly_ip_obj = NetAddr::IP->new($rly_ip);  # TCP-info field
#                                    if (!defined $rly_ip_obj) {
#                                        do_log($ll, "rules: bad IP address in relay: " . $rly_ip . " sender: " . $sender . "from: " . $from);
#                                    } else {
#                                        my $wl_ip_obj = NetAddr::IP->new($wl_ip); # whitelist 2nd param
#                                        if (!defined $wl_ip_obj) {
#                                            do_log($ll, "rules: bad IP address in whitelist: " . $wl_ip);
#                                        } elsif ($wl_ip_obj->contains($rly_ip_obj)) {
#                                            # note: an IPv4-compatible IPv6 address can match an IPv4 addr
#                                            do_log($ll, "rules: relay addr " . $rly_ip . " matches whitelist " . $wl_ip_obj .  " sender: " . $sender . " from: " . $from);
#                                            $match = 1; last;
##                                        } else {
#                                            do_log($ll, "rules: relay addr " . $rly_ip . " does not match wl " . $wl_ip_obj . " sender " . $sender . " from: " . $from);
#                                        }
#                                    }
#                                }
#                                
#                            } else {  # match by a rdns name
#                                my $rdns = $hostname;
#                                if ($rdns =~ /(?:^|\.)\Q${matchdomain}\E$/i) { $match=1; last }
#                            }
#                        }
#                        if ($match) {
#                            if ($wl) {
#                                $r->recip_whitelisted_sender(1);
#                                do_log($ll,"CUSTOM: whitelist_from_rcvd '$matchaddr $matchdomain'") if ($match);
#                            	push @hdrinfo , "whitelist_from_rcvd: '$matchaddr $matchdomain'";
#                            } else {
#                                $r->recip_blacklisted_sender(1);
#                                do_log($ll,"CUSTOM: blacklist_from_rcvd '$matchaddr $matchdomain'") if ($match);
#                            	push @hdrinfo , "blacklist_from_rcvd: '$matchaddr $matchdomain'";
#
#                            }
#                        }
#                    }
#                }
#TODO testme
                elsif ( (/^whitelist_subject/) || (/^blacklist_subject/) ) { 
                    my $wl = (/^whitelist_subject/) ? 1 : 0;
                    my $re = lc($value); 
                    do_log($ll,"CUSTOM: calling white/blacklist_subject ($re)");
        
                    $re =~ s/[\000\\\(]/_/gs;                   # paranoia
                    $re =~ s/([^\*\?_a-zA-Z0-9])/\\$1/g;        # escape any possible metachars
                    $re =~ tr/?/./;                             # "?" -> "."
                    $re =~ s/\*+/\.\*/g;                        # "*" -> "any string"
                    my $subject = lc($subj);
                    if ($subject =~ qr/$re/i) {
                        if ($wl) {
                            $r->recip_whitelisted_sender(1);
                            do_log($ll,"CUSTOM: whitelist_subject: '$re'");
                            push @hdrinfo , "whitelist_subject: '$re'";
                        } else {
                            $r->recip_blacklisted_sender(1);
                            do_log($ll,"CUSTOM: blacklist_subject: '$re'");
                            push @hdrinfo , "blacklist_subject: '$re'";
                        }
                    }
                }

#TODO testme
                elsif ( (/^whitelist_header/) || (/^blacklist_header/) ) {
                    my $wl = (/^whitelist_header/) ? 1 : 0;
                    my ($wblhdr, $re) = split(/ /, lc($value), 2);
                    do_log($ll,"CUSTOM: calling white/blacklist_header ($re)");
                    my($hdr) = $msginfo->get_header_field_body($wblhdr);
                    if ($hdr) {
                        $re =~ s/[\000\\\(]/_/gs;                   # paranoia
                        $re =~ s/([^\*\?_a-zA-Z0-9])/\\$1/g;        # escape any possible metachars
                        $re =~ tr/?/./;                             # "?" -> "."
                        $re =~ s/\*+/\.\*/g;                        # "*" -> "any string"
                         if ($hdr =~ qr/$re/i) {
                            if ($wl) {
                                $r->recip_whitelisted_sender(1);
                                do_log($ll,"CUSTOM: whitelist_header: '$wblhdr $re'");
                                push @hdrinfo , "whitelist_header: '$wblhdr $re'";
                            } else {
                                $r->recip_blacklisted_sender(1);
                                do_log($ll,"CUSTOM: blacklist_header: '$wblhdr $re'");
                                push @hdrinfo , "blacklist_header: '$wblhdr $re'";
                            }
                        }
                    }
                }

            } # end for preference
        } # end while

        $conn_h->finish($userpref_query)  if defined $a_ref;  # only if not all read

	if (scalar @score_rules) {
        	# now that the other things have run, loop through and apply score rules.
        	foreach my $s (@score_rules) {  
            		my $preference = $s->[0];
            		my $value = $s->[1];
            		my ($ignore, $rulename) = split (/ /, $preference); 
            		my $scoreadjust = $value;
            		if ( $scores{$rulename} ) {
                		$scores{$rulename}=$value;
                		do_log($ll,"CUSTOM: custom score match($rulename, $scoreadjust)");
                		push @hdrinfo , "score: '$rulename, $scoreadjust'";
            		}
        	}

        	my $newscorestr = join(q{,}, map{qq{$_=$scores{$_}}} sort keys %scores);
        	do_log($ll,"CUSTOM: original \$r->spam_tests = " . $r->spam_tests);
        	$r->spam_tests([ \$newscorestr]);
        	do_log($ll,"CUSTOM: setting \$r->spam_tests to '$newscorestr'");
       	 
        	my $total;
        	while ( my ($key, $val)  = each %scores ) {
            		$total += $val;
        	}
        	do_log($ll,"CUSTOM: original \$r->spam_level =  " . $r->spam_level);
        	$r->spam_level( $total );
        	do_log($ll,"CUSTOM: setting \$r->spam_level to '$total'");

        	my($hdr_edits) = $msginfo->header_edits;
		$hdr_edits->add_header('X-Spam-Custom', join ', ', @hdrinfo) if @hdrinfo;
	}
    }
  }
}


1;  # insure a defined return



