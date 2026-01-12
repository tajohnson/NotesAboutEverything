
## what needs to move to feature_settings table?
- Continuity
  - [ ] retention_continuity
- Hosted Mail
  - [ ] quota_mb
- Filter settings
- [ ] x 
- Autoresponder settings
- [ ] Subject
- [ ] body_plain
- [ ] body_html
- [ ] start_date
- [ ] end_date
- [ ] keep original
- Mangler
- [ ] x
  - hosting?

### notes:

### dovecot:

```
OLD:

iterate_query = select distinct(concat(e.localpart, '@', d.name))  as username from auth_user u    left join amavisd_emailaccount e on u.id=e.user_id    left join amavisd_localpartalias la on e.id=la.email_account_id    left join amavisd_domain d on la.domain_id=d.id   left join temporary_hosted h on d.id=h.domain_id where (h.continuity=1 OR h.hosted=1)

NEW:
# find all users with either hosted or continuity enabled
select distinct(concat(e.localpart, '@', d.name))  as username from auth_user u left join amavisd_emailaccount e on u.id=e.user_id left join amavisd_localpartalias la on e.id=la.email_account_id left join amavisd_domain d on la.domain_id=d.id left join amavisd_policy p on d.policy_id=p.id where p.hosted_email_enabled in ('y', 'Y') OR p.continuity_enabled in ('y', 'Y');


```

```
OLD:

user_query = \
SELECT home, mail, userdb_import, quota_rule FROM\
  (SELECT \
  concat("/data/mailstorage/", \
      substring(d.name,1, 1), "/", substring(d.name,2, 1), "/", d.name, "/", \
      substring(e.localpart,1, 1), "/", e.localpart) as home, \
  concat("maildir:/data/mailstorage/", \
      substring(d.name,1, 1), "/", substring(d.name,2, 1), "/", d.name, "/", \
      substring(e.localpart,1, 1), "/", e.localpart, \
      "/mail:INDEX=/data/mailindices/", \
      substring(d.name,1, 1), "/", substring(d.name,2, 1), "/", d.name, "/", \
      substring(e.localpart,1, 1), "/", e.localpart, \
      ":LAYOUT=fs") as mail, \
  IF (h.continuity, \
       CONCAT( \
       'namespace/inbox/mailbox/inbox/autoexpunge=', h.cont_storage_days, "d\t",       \
       'namespace/inbox/mailbox/Drafts/autoexpunge=', h.cont_storage_days, "d\t",       \
       'namespace/inbox/mailbox/Quarantine/autoexpunge=', h.cont_storage_days, "d\t",   \
       'namespace/inbox/mailbox/Quarantine.Spam/autoexpunge=', h.cont_storage_days, "d\t", \
       'namespace/inbox/mailbox/Quarantine.BadHeaders/autoexpunge=', h.cont_storage_days, "d\t", \
       'namespace/inbox/mailbox/Quarantine.BannedFiles/autoexpunge=', h.cont_storage_days, "d\t", \
       'namespace/inbox/mailbox/Quarantine.Blocked/autoexpunge=', h.cont_storage_days, "d\t", \
       'namespace/inbox/mailbox/Quarantine.Virus/autoexpunge=', h.cont_storage_days, "d\t", \
       'namespace/inbox/mailbox/Trash/autoexpunge=', h.cont_storage_days, "d\t",        \
       'namespace/inbox/mailbox/Sent/autoexpunge=', h.cont_storage_days, "d"), '')      \
as userdb_import,\
  IF (h.continuity, 'continuity', '') as acl_groups,\
        CONCAT('*:storage=', quota_mb, 'M') AS quota_rule \
  FROM auth_user u LEFT JOIN amavisd_emailaccount e ON u.id=e.user_id \
    LEFT JOIN amavisd_localpartalias la ON e.id=la.email_account_id \
    LEFT JOIN amavisd_domainalias da ON la.domain_id=da.domain_id  \
    LEFT JOIN amavisd_domain d ON la.domain_id=d.id  \
    LEFT JOIN temporary_hosted h ON da.domain_id=h.domain_id \
  WHERE (la.localpart='%n' OR la.localpart = substring_index('%n','-', 1) OR la.localpart = substring_index('%n','+', 1)  ) and (da.name = '%d' OR la.external_domain = '%d')  and (h.hosted=1 or h.continuity=1)) dt


NEW:

user_query = \
SELECT home, mail, userdb_import, quota_rule FROM
  (SELECT 
  concat("/data/mailstorage/", 
      substring(d.name,1, 1), "/", substring(d.name,2, 1), "/", d.name, "/", 
      substring(e.localpart,1, 1), "/", e.localpart) as home, 
  concat("maildir:/data/mailstorage/", 
      substring(d.name,1, 1), "/", substring(d.name,2, 1), "/", d.name, "/", 
      substring(e.localpart,1, 1), "/", e.localpart, 
      "/mail:INDEX=/data/mailindices/", 
      substring(d.name,1, 1), "/", substring(d.name,2, 1), "/", d.name, "/", 
      substring(e.localpart,1, 1), "/", e.localpart, 
      ":LAYOUT=fs") as mail, 
  IF (LOWER(p.continuity_enabled) = 'y', 
       CONCAT( 
       'namespace/inbox/mailbox/inbox/autoexpunge=', s.settings->>'$.continuity.quota_mb', "d\t",
       'namespace/inbox/mailbox/Drafts/autoexpunge=', s.settings->>'$.continuity.quota_mb', "d\t",
       'namespace/inbox/mailbox/Quarantine/autoexpunge=', s.settings->>'$.continuity.quota_mb', "d\t",
       'namespace/inbox/mailbox/Quarantine.Spam/autoexpunge=', s.settings->>'$.continuity.quota_mb', "d\t",
       'namespace/inbox/mailbox/Quarantine.BadHeaders/autoexpunge=', s.settings->>'$.continuity.quota_mb', "d\t",
       'namespace/inbox/mailbox/Quarantine.BannedFiles/autoexpunge=', s.settings->>'$.continuity.quota_mb', "d\t",
       'namespace/inbox/mailbox/Quarantine.Blocked/autoexpunge=', s.settings->>'$.continuity.quota_mb', "d\t",
       'namespace/inbox/mailbox/Quarantine.Virus/autoexpunge=', s.settings->>'$.continuity.quota_mb', "d\t",
       'namespace/inbox/mailbox/Trash/autoexpunge=', s.settings->>'$.continuity.quota_mb', "d\t",
       'namespace/inbox/mailbox/Sent/autoexpunge=', s.settings->>'$.continuity.quota_mb', "d"), '')
  as userdb_import,
  IF (LOWER(p.continuity_enabled) = 'y', 'continuity', '') as acl_groups,
  CONCAT('*:storage=', s.settings->>'$.continuity.quota_mb', 'M') AS quota_rule
  FROM auth_user u 
    LEFT JOIN amavisd_emailaccount e ON u.id = e.user_id 
    LEFT JOIN amavisd_localpartalias la ON e.id = la.email_account_id 
    LEFT JOIN amavisd_domainalias da ON la.domain_id = da.domain_id  
    LEFT JOIN amavisd_domain d ON la.domain_id = d.id  
    LEFT JOIN amavisd_policy p ON d.policy_id = p.id 
    LEFT JOIN feature_settings s ON s.domain_id = d.id 
  WHERE (la.localpart = '%n' 
    OR la.localpart = substring_index('%n','-', 1) 
    OR la.localpart = substring_index('%n','+', 1)) 
    AND (da.name = '%d' OR la.external_domain = '%d')  
    AND (LOWER(p.hosted_email_enabled) = 'y' OR LOWER(p.continuity_enabled) = 'y')) dt
    
```