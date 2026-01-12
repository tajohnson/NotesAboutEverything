package Amavis::Custom;
use strict;

##
## Amavis custom hook to save incoming bulk/autoreply sender addresses
## in database and discard autoreply messages outgoing to those addresses
##
## Load from amavisd.conf: include_config_files('autoreply.conf');
##
## CREATE TABLE bulk (
##   email varchar(255) NOT NULL UNIQUE,
##   time_iso TIMESTAMP NOT NULL DEFAULT 0
## );
##
## Crontab a cleanup every hour or so:
## DELETE FROM bulk WHERE time_iso < DATE_SUB( UTC_TIMESTAMP(), INTERVAL 1 HOUR );
##
## Set a rule to MTA to discard messages with X-Amavis-Discard header
##
## hege@hege.li / Version 0.07 / Public Domain
##

# Database
my @db = ( ['DBI:mysql:database=amavis;host=localhost', 'amavis', 'xxx'] );

# Subjects that are considered autoreplies
my $autoreply_subjects =
    qr/^((?:
	Delivery\ Status\ Notification |
	Out\ Of\ Office\ AutoReply |
	(?:Ei\ )?Luettu: |
	(?:Not\ )?Read: |
        Auto(?: matic\ out-of-office | matisch\ antwoord | m\.\ vast |
	        maattinen\ poissaolovastaus | mated\ reply |
                maattinen\ vastaus | maattinen\ vastaanottokuittaus |
                svar\ -\ Ikke\ til\ stede )\b |
        Kiitos\ yhteydenotostanne!$
    ).*)/ix;

# Don't reply when any of these SpamAssassin rules are hit
my $bulk_spam_rules = 
    qr/(?:^|[\s,])
	( DCC_CHECK | RAZOR2_CHECK | IXHASH | BAYES_[89]. | AV[:.][^=]+ )
    =/x;

# Don't reply when SpamAssassin score is over
my $bulk_spam_level = 2;

# Generic list of senders emails that we never want to autoreply
my $bulk_senders =
    qr/^(?:[^\@]+-(?:request|bounces?|admin|owner) |
	(?:sentto|owner|return|(?:gr)?bounce)-[^\@]+ |
        [^\@]*\b(?:do[._-t]?)?no[t._-]?repl(?:y|ies) |
        mailer-daemon | postmaster
    )\@/ix;

# Insert email SQL
my $insert_clause =
	"INSERT INTO bulk VALUES (?, UTC_TIMESTAMP()) ".
        "ON DUPLICATE KEY UPDATE time_iso = UTC_TIMESTAMP()";

# Check email SQL
my $select_clause =
	"SELECT email FROM bulk WHERE email=?";


##
##
##

use DBI qw(:sql_types);
use DBD::mysql;

BEGIN {
  import Amavis::Conf qw(:platform);
  import Amavis::Util qw(do_log untaint safe_decode);
}

sub new {
  my($class,$conn,$msginfo) = @_;
  my($self) = bless {}, $class;
  my($conn_h) = Amavis::Out::SQL::Connection->new(@db);
  $self->{'conn_h'} = $conn_h;
  $self;
}

sub is_subject_oof {
  my($subj) = shift;

  if (defined $subj and $subj =~ $autoreply_subjects) {
    do_log(2,"CUSTOM_autoreply: Autoreply subject = %s", $1);
    return 1;
  }

  return undef;
}

sub is_bulk_spam_rule {
  my($s) = shift;

  if (defined $s and $s =~ $bulk_spam_rules) {
    do_log(2,"CUSTOM_autoreply: Bulk rule = %s", $1);
    return 1;
  }

  return undef;
}

sub dec {
  my($str) = shift;
  return unless defined $str or $str ne '';

  chomp $str;
  $str =~ s/\n([ \t])/$1/sg; $str =~ s/^[ \t]+//s; $str =~ s/[ \t]+\z//s;
  my($str2);
  eval { $str2 = safe_decode('MIME-Header',$str) };
  $@ eq '' ? return $str2 : return $str;
}

sub before_send {
  my ($self,$conn,$msginfo) = @_;

  ## Skip if already blocked
  return if $msginfo->blocking_ccat;

  my $r = @{$msginfo->per_recip_data}[0];
  my $recipient = lc(untaint($r->recip_addr));

  my $autoreply = 0;
  my $bulk = 0;
  my $spammy = 0;

  if (is_subject_oof(dec($msginfo->get_header_field_body('subject')))) {
    $autoreply = 1;
  }
  elsif (defined $msginfo->get_header_field_body('x-autoreply')) {
    do_log(2,"CUSTOM_autoreply: X-Autoreply header");
    $autoreply = 1;
  }
  elsif (defined $msginfo->get_header_field_body('x-auto-response-suppress') &&
         $msginfo->get_header_field_body('x-auto-response-suppress') =~ /\b(All|OOF)\b/i) {
    do_log(2,"CUSTOM_autoreply: X-Auto-Response-Suppress header ($1)");
    $autoreply = 1;
  }

  if ($msginfo->is_bulk) {
    do_log(2,"CUSTOM_autoreply: Amavis set is_bulk");
    $bulk = 1;
  }
  elsif ($msginfo->sender_smtp eq '<>') {
    do_log(2,"CUSTOM_autoreply: Null sender");
    $bulk = 1;
  }
  elsif ($msginfo->sender =~ $bulk_senders) {
    do_log(2,"CUSTOM_autoreply: Bulk smtp sender");
    $bulk = 1;
  }

  my $score = $r->spam_level;
  if ($score > $bulk_spam_level) {
    do_log(2,"CUSTOM_autoreply: Spam score over %d (%d)", $bulk_spam_level, $score);
    $spammy = 1;
  }
  else {
    my $tests = $r->spam_tests;
    if (defined $tests && is_bulk_spam_rule( join(',', map {$$_} @$tests) )) {
      $spammy = 1;
    }
  }

  ## Skip if nothing matched
  return unless ($autoreply || $bulk || $spammy);

  ## Autoreplies are not usually multirecipient..
  return if ($autoreply && @{$msginfo->per_recip_data} > 1);


  ###
  ### Originating
  ###

  if ($msginfo->originating) {

    ## Skip if internal mail
    return if $r->recip_is_local;

    ## Drop to bulk senders
    if ($recipient =~ $bulk_senders) {
      do_log(2,"CUSTOM_autoreply: Setting X-Amavis-Discard");
      $msginfo->header_edits->add_header('X-Amavis-Discard', 'Yes');
      return;
    }

    ## Check if sender is in bulk database
    my($conn_h) = $self->{'conn_h'};
    $conn_h->begin_work_nontransaction;
    my(@pos_args) = ( $recipient );
    $conn_h->execute($select_clause, @pos_args);
    my($a_ref);
    if (defined($a_ref=$conn_h->fetchrow_arrayref($select_clause))) {
      do_log(2,"CUSTOM_autoreply: Setting X-Amavis-Discard");
      $msginfo->header_edits->add_header('X-Amavis-Discard', 'Yes');
    }
    $conn_h->finish($select_clause) if defined $a_ref;

    return;
  }


  ###
  ### Incoming
  ###

  ## Save bulk sender in database
  my(@rfc2822_from) = do {my $f = $msginfo->rfc2822_from; ref $f ? @$f : $f};
  push(@rfc2822_from, dec($msginfo->get_header_field_body('reply-to')));
  my(@addr_list) = map {lc} map {split /, /} ($msginfo->sender, $msginfo->rfc2822_sender, @rfc2822_from);
  my %addr_uniq;
  for (@addr_list) {
	s/.*<([^>]+)>.*/$1/;
	next if $_ =~ $bulk_senders;
	$addr_uniq{$_} = 1 if /@/;
  }
  return unless scalar keys %addr_uniq;

  foreach (keys %addr_uniq) {
    do_log(2,"CUSTOM_autoreply: Bulk sender = %s", $_);
  }

  my($conn_h) = $self->{'conn_h'};
  $conn_h->begin_work_nontransaction;
  foreach my $from (keys %addr_uniq) {
    my(@pos_args) = ( untaint($from) );
    $conn_h->execute($insert_clause, @pos_args);
  }
  #$conn_h->commit;
}

1;