# TODO - Work (MailRoute)

## URGENT

- [x] **Miami move to Cogent** - fix NS records, add MIA back, add back cloudns, figure out letsencrypt
- [ ] Update stalwart to 0.13.4 with enterprise feature enabled (already built but binary not copied over ansible.  Probably need to update binaries manually, since all have to be stopped, replaced, and then started - I think ansible might not be the way to do this)
- [ ] Replace RAM in 013.lax
- [ ] CHECK CERTBOT - make sure certs are being copied when updated


- [ ] Get 005.mia and 006.mia processing mail - add slowly - these are new IPs and need to be warmed up

- [ ] Move hostedmail onto es01 through es0x in MIA - move foundationdb there,  etc...
- [ ] Clear out MERCK stuff

---

## Amazon RDS

- [x] Updated all database minor versions (2025-08-30)
- [x] Added replica in us-east-2, updated DNS on 103
- [x] Removed old replica mroute-global-instance-1-us-east-1-1
- [ ] Delete: "mroute-global-cluster-us-east-1", "mroute-global-instance-1-us-east-1-1"
- [ ] Add back read replica for MIA to use
- [ ] Consider: does east read replica actually help? Maybe use provisioned instance for dev instead?

---

## Hosted Mail Launch

### High Priority
- [x] Further test of hook
- [x] Move production data into new db structures and test hook
	- [ ] On es01.lax in ~root - migrate_to_new_wblist_extended.py does whitelist/blacklist
- [x] Resolve distribution list handling (within Stalwart or internal forwarding)
- [ ] Should we do imapsync feature for users to migrate from old IMAP server?

### UI/Docs
- [x] Create UI pages for hosted mail
- [x] Include mobileconfig and related info on hosted mail pages
- [x] Rewrite KB articles in SMTP Auth to cover hosted mail

### Marketing
- [ ] Prepare press release
- [ ] Draft notification for resellers

### Post-Launch
- [ ] Plan migration of entries from old whitelist/blacklist to new system

---


---

## Server Upgrades - Alma 9.3 to 9.5

Network breaks on upgrade - use the save/restore scripts from 199.89.0.103

- [ ] 001.lax
- [ ] 002.lax - careful, it's control panel
- [ ] 003.lax
- [ ] 004.lax
- [ ] 005.lax
- [ ] 006.lax
- [ ] 007.lax
- [ ] 008.lax
- [ ] 009.lax
- [ ] 010.lax
- [ ] 011.lax
- [ ] 012.lax
- [ ] 013.lax
- [ ] 014.lax
- [ ] 001.mia
- [ ] 002.mia
- [ ] 003.mia
- [ ] 004.mia

After upgrades:
- [ ] Update spamassassin to 4.0.1
- [ ] Test QRCode.pm plugin on 001.lax (/etc/mail/spamassassin/plugins/QRCode.pm)
- [ ] Consider elasticsearch rolling upgrade

---

## Infrastructure

### MIA Datacenter
- [ ] Get 005.mia and 006.mia processing mail (new IPs need warming)
- [ ] Move hostedmail onto es01-es0x in MIA, move foundationdb there
- [ ] Consider moving MIA to San Diego to reduce latency to LAX

### Certbot
- [ ] Make sure ssh-copy-id working with all servers
- [ ] Add cert expiration check - warn if < 30 days

### Postfix/IMAP
- [ ] Set up and test imapfake (199.89.x.x for imap.mailroute.net pointing to fake imap for ACM)
- [ ] Set up stalwart on new addresses (199.89.x.131 = hostedmail.mailroute.net)

### Redis
- [ ] Consider moving from redis to KeyDB (or something else?) in master-master config (after MIA closer)
- [ ] Move redis off s02.lax.int to 012.lax.int (already set up there for decode urls)
- [ ] Update logstash after redis move

### Other
- [ ] Put another control panel instance in MIA and load-balance
- [ ] Migrate servers 010, s01, s02 to alma9 (remaining)
- [ ] Get p0f working again (requires transparent mode in haproxy)
- [ ] Change ES shards to 4, replica to 1 on new cluster
- [ ] Reserved instances for AWS (after quarantine shut down)
- [ ] move redis to aws instance or some other hosted service? Same with memcached?  Or use a bare metal server somewhere else for this?
- [ ] Move qa, dev databases to new, separate aws instances, for ease of snapshotting, backup/restore
- [ ] memcached - run separately in each datacenter - only used for caching recipient accss verification info

---

## Feature Ideas & Specs

See [[Features/Feature Ideas]] for backlog items and feature specifications.

---

## Completed / Done

- [x] AWS migration
- [x] ES/S3 delivery agent
- [x] Set up 002, 005 as new web app servers for ui2
- [x] Monitoring setup (prometheus)
- [x] Private network between haproxy and filtering servers
- [x] Stalwart upgrade and webadmin behind nginx
- [x] Quota management for Hosted Mail
- [x] Switch smtp-auth to dovecot
- [x] DNSSec for mailroute.net (basic setup)
