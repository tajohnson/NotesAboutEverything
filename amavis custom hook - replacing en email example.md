package Amavis::Custom;
use strict;

BEGIN {
  import Amavis::Conf qw(:platform :confvars c cr ca);
  import Amavis::Util qw(do_log untaint safe_encode safe_decode);
}

use IO::File qw(O_RDONLY O_WRONLY O_RDWR O_APPEND O_CREAT O_EXCL);

sub new {
  my($class,$conn,$msginfo) = @_;
  my($self) = bless {}, $class;
  $self;
}

sub checks {  # may be left out if not needed
  my($self,$conn,$msginfo) = @_;
  my($out_fh) = IO::File->new;
  my($repl_fn) = $msginfo->mail_tempdir . '/email-replxx.txt';
  do_log(2, "CUSTOM HERE: %s", $repl_fn);
  $out_fh = IO::File->new;
  $out_fh->open($repl_fn, O_CREAT|O_EXCL|O_RDWR, 0640)
    or die "Can't create file $repl_fn: $!";
  binmode($out_fh,":bytes") or die "Can't cancel :utf8 mode: $!";
  print $out_fh "From: Mark.Martinec\@ijs.si\n";
  print $out_fh "To: Mark.Martinec\@ijs.si\n";
  print $out_fh "Subject: test\n";
  print $out_fh "\n";
  print $out_fh "test\n";
  $out_fh->close or die "Can't close $repl_fn: $!";

  my($new_fh) = IO::File->new;
  $new_fh->open($repl_fn,'<') or die "Can't open file $repl_fn: $!";
  unlink($repl_fn) or die "Can't unlink file $repl_fn: $!";
  binmode($new_fh,":bytes")
    or die "Can't set binmode on $repl_fn: $!";
  $msginfo->mail_text($new_fh);   # substitute mail with rewritten version
  $msginfo->mail_text_fn(undef);  # remove filename information
}

