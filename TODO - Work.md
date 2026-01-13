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
- [x] Reserved instances for AWS (after quarantine shut down)
- [ ] move redis to aws instance or some other hosted service? Same with memcached?  Or use a bare metal server somewhere else for this?
- [ ] Move qa, dev databases to new, separate aws instances, for ease of snapshotting, backup/restore
- [ ] memcached - run separately in each datacenter - only used for caching recipient accss verification info

---

## Infrastructure Projects

### Netdata Monitoring
Spec: `20-NETDATA-MONITORING.md` | Status: Phases 1-4.8 complete
- [ ] Policyd custom monitoring (charts.d plugin)
- [ ] Stalwart IMAP custom monitoring (charts.d plugin)
- [ ] Unbound monitoring (go.d/unbound - depends on Unbound deployment)

### HAProxy Capacity Weights
Spec: `30-HAPROXY-CAPACITY-WEIGHTS.md` | Status: Ready to implement
- [ ] Replace static HAProxy weights with capacity-based weights from server hardware (physical_cores)
- [ ] Extend lb_all dictionary with capacity info
- [ ] Update haproxy.cfg.j2 template to calculate weights automatically

### IP Warmup Strategy
Spec: `31-IP-WARMUP-STRATEGY.md` | Status: Architecture defined
- [ ] Implement two-tier relay architecture (separate filtering from delivery)
- [ ] Configure HAProxy weights on delivery pool for IP warmup control
- [ ] Apply to 005.mia, 006.mia, 009.lax, 012.lax, 008.lax

### Architecture Documentation
Spec: `40-ARCHITECTURE-DOCUMENTATION.md` | Status: Phase 1 in progress (ARCH-00-OVERVIEW complete)
- [ ] ARCH-01: Infrastructure (datacenters, servers, network)
- [ ] ARCH-02: Email Flow (filtering pipeline)
- [ ] ARCH-03: Load Balancing (HAProxy, keepalived)
- [ ] ARCH-04: DNS (BIND, zones, DNSBL)
- [ ] ARCH-05: Databases (Aurora, Redis, memcached)
- [ ] ARCH-06: Security (auth, encryption, secrets)
- [ ] ARCH-07: Monitoring (Netdata, ELK, alerting)

### MkDocs Knowledge Base Migration
Spec: `50-MKDOCS-KB-MIGRATION.md` | Status: Planning complete
- [ ] Phase 1: Ansible deployment (nginx serving static files)
- [ ] Phase 2: GitHub repo + MkDocs setup + GitHub Actions CI/CD
- [ ] Phase 3: Zendesk export (Python script, HTML→Markdown)
- [ ] Phase 4: Content reorganization
- [ ] Phase 5: Theming & branding
- [ ] Phase 6: Go live

### Quarantine Retention Bypass
Spec: `50-QUARANTINE-RETENTION-BYPASS.md` | Status: Planned
- [ ] Skip S3 storage when retention_quarantine = 0 (Education plans, FERPA compliance)

### Unbound Local Caching
Spec: `50-UNBOUND-LOCAL-CACHING.md` | Status: Ready for implementation
- [ ] Deploy Unbound as local caching DNS on non-BIND servers (load balancers, search, IMAP)
- [ ] Configure forwarding to central BIND servers

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
