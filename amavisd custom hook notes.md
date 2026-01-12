> I'd like to reset the contents_category to CC_CLEAN in the case where my
> own whitelist has cleared the email. Is that possible?
> I'm able to get the current status of the email from per_recip_data and
> contents_category, where I can see that the email has been flagged in
> CC_CLEAN and CC_BANNED ( 1 and 8 ).

Something like this should do:

    $msginfo->contents_category(undef);
    $msginfo->add_contents_category(CC_CLEAN,0);

    for my $r (@{$msginfo->per_recip_data}) {
      $r->contents_category(undef);
      $r->add_contents_category(CC_CLEAN,0);
    }







> There is amavis + postfix on Linux box. And we have a whitelist  like
> "whitelist_from bla-bla-bla" and spamassassin adds -100 to the score if
> a sender from whitelist.
>
> And now the question is: if I'd like to send a copy of any message with
> score < -100 to nospam@my-smtproxy.domain1 to have processed it with
> bayes later, it's possible?

Perhaps the cleanes is by using a custom hook:

/etc/amavisd.conf:
  include_config_files('/etc/amavisd-custom-test.conf');


/etc/amavisd-custom-test.conf :


package Amavis::Custom;
use strict;

BEGIN {
  import Amavis::Conf qw(:platform :confvars c cr ca $myhostname);
  import Amavis::rfc2821_2822_Tools;
  import Amavis::Util qw(do_log untaint);
}

sub new {
  my($class,$conn,$msginfo) = @_;
  my($self) = bless {}, $class;
  $self;
}

sub after_send {
  my($self,$conn,$msginfo) = @_;
  my($spam_level) = $msginfo->spam_level;
  do_log(1,"CUSTOM: entered after_send hook");
  if ($spam_level < -100) {
    my($recip) = 'autolearner@example.com';
    do_log(1,"CUSTOM: sending a copy to %s", $recip);
    my($notification) = Amavis::In::Message->new;
    $notification->rx_time($msginfo->rx_time);  # copy the reception time
    $notification->log_id($msginfo->log_id);    # copy log id
    $notification->msg_size($msginfo->msg_size);
    $notification->delivery_method(c('notify_method'));
    $notification->originating(0);  # disables DKIM signing
    $notification->sender('');  # use null or whatever sender address
    $notification->sender_smtp(qquote_rfc2821_local($notification->sender));
    $notification->recips([$recip]);
    $notification->mail_text($msginfo->mail_text);  # use the same contents
    Amavis::mail_dispatch($conn, $notification, 'Quar', 0);
    my($n_smtp_resp, $n_exit_code, $n_dsn_needed) =
      one_response_for_all($notification, 0);  # check status
    if ($n_smtp_resp =~ /^2/ && !$n_dsn_needed) {  # ok
    } elsif ($n_smtp_resp =~ /^4/) {
      die "temporarily unable to alert recipient: $n_smtp_resp";
    } else {
      do_log(-1, "FAILED to send a copy: %s", $n_smtp_resp);
    }
  }
}

1;  # insure a defined return






