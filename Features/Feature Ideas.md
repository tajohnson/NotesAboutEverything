# Feature Ideas (Backlog)

Ideas and potential features for MailRoute. See individual feature files in this directory for detailed specs.

---

## Security
- [ ] DLP rules (credit cards, SSNs) for inbound/outbound
- [ ] Speed bumps for links/downloads (delay until AV engines catch up)
- [ ] Per-customer encryption option
- [ ] Extra warnings when releasing high-score/dkim-fail emails
- [ ] Undeliver mail in O365/IMAP (remove from inbox)
- [ ] Info bar with security score (A-F based on dkim, tls, score)
- [ ] Secure email (paubox-style) - redirect non-TLS to secure site
- [ ] first time sender warning?
- [ ] Check phishing domains before letting someone whitelist a domain?  Ie gmailmsg.com is a known phishing site
- [ ] Secure email - see example at https://www.mailprotector.com/encryption/
   			idea? Could you enhance the security of the email by requiring 2fa from SMS? Like the sender includes the phone number of the recipient as well?

---

## SpamAssassin
- [ ] spamassassin plugin that counts number of freemail accounts in To headers.
- [ ] New filter option - send direct to qtn?
      Option: all bulk mail to qtn
      	score RCVD_IN_MR_SERVICE_BULK high

      Option: all txl mail to qtn
      	score RCVD_IN_MR_SERVICE_TXL high

      Option: block dsn
      	score ANY_BOUNCE_MESSAGE high

---

## Delivery
- [ ] enable some continuity for all customers by default.  Give opt out option on domain and/or user basis.  This would let us grab a recently delivered email to use for bates retraining or support. Needs to be secure, though.  Customer needs to know what's being stored.
- [ ] route all unfiltered mail out through non-mailroute IP address space (ie ACM forwards).
- [ ] give domains option to set TLS state (none, may, encrypt) for their inbound relay.  (see [[TLS Relay for Inbound]])

---

## Quarantine
- [ ] New wblist match actions: Bounce, Hold for Review, Address Alteration, BCC, Strip attachments
- [ ] Group of admins to review/release/reject held emails
- [ ] Different qtn notification for TXL/Bulk email
- [ ] Select what message types appear in quarantine report
- [ ] make qtn messages more configurable. What categories are displayed, etc.  Maybe can create different "views"?  Maybe a qtn message building?
- [ ] New actions for wblist?  See [[New Actions for WBList]]
- [ ] Require wlist to use ip/dkim/etc?
- [ ] don't let people recover dkim fails, or raise a warning!
- [ ] new tab in quarantine for Spoofing documents.  Maybe different warnings, maybe different warnings when recovering emails?
- [ ] maybe stop or warn on recovering email without dkim/spf/dmarc, or obvious high scoring email
- [ ] categorize marketing and transactional email, and maybe block them, or show them in a new multipart qtn notification that lists those emails.  Maybe it could have a preview of each message? But try to recognize things like password resets, 2fa requests, etc and let those through.

---

## User Features
- [ ] IMAP access to quarantine/continuity
- [ ] Allow users to unsubscribe from txl/bulk from continuity
- [ ] Replay email stream to server (last X minutes/hours)
- [ ] "Second Inbox" concept - only important mail to work inbox, rest in continuity
- [ ] IMAP feature built into user app (web and phone), to allow users to view their email in our own client (we would have to be sure we build a GREAT email client).  We could also then create a Reported Spam folder on their IMAP server and read anything they put in there.
- [ ] Once we have the "user" app (web and mobile), consider showing log info of deliveries of legit messages in there, and allow users to "unsubscribe" from legit opt-in mailings....what else could we do there?

---

## Admin Features
- [ ] Show how filter setting changes would have affected last 2 weeks of traffic
- [ ] Reports of users who release/whitelist most
- [ ] Internal reports to find customers leaving (new admins, billing changes)
- [ ] Periodic emailed reports (monthly, annual)
- [ ] Admin phone verification code
- [ ] admin can give permissions to recover different types of email, based on score, or dkim fail, or non-tls, etc.
- [ ] using o365 api, let an admin remove a message from his user's emailboxes.  maybe from the maillog?  Could this be done with imap access???
- [ ] let admin choose that when user releases email, it goes to admin, not to end-user.
- [ ] integrated helpdesk within control panel

---

## Email Modification
- [ ] Link protection/scanning (rewrite URLs, time-of-click protection)
- [ ] Attachment sandboxing
- [ ] Customized disclaimers/signatures
- [ ] Time-based email expiration
- [ ] AI content summarization
- [ ] Language translation
- [ ] maybe in addition to info bar - info in header. Write plugins for different email clients to display it????

---

## Marketing / Onboarding
- [ ] AI: can Claude help develop prompt for doing SEO for marketing site? Cannot evaluate competitors to help?
- [ ] Automate sending out regular reporting emails to admins.  Make it possible to add some custom stuff to each message for marketing/retention
- [ ] Emails to customer on signup
     * @ time of signup: Welcome to MailRoute
     * @ 3 days: How can we help you?
     * @ 7 days: Did you know? Hints, plus stats
     * @ 10 days: trial is almost up.  We'll be sending you an invoice on x date,

---

## Detailed Feature Specs

See these files for more detailed specifications:

- [[MTA-STS Support]]
- [[Archiving Service]]
- [[Reporting and Metrics]]
- [[Message Store Service]]
- [[IMAP Xfer]]
- [[New Actions for WBList]]
- [[TLS Relay for Inbound]]
