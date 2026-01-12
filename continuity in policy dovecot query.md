dovecot query changed to support the move from the temporary_hosted table to the policyd table:



SELECT home, mail, userdb_import, NULL as quota_rule FROM\
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
  IF (p.continuity_enabled, \
       CONCAT( \
       'namespace/inbox/mailbox/inbox/autoexpunge=', p.retention_continuity, "d\t",       \
       'namespace/inbox/mailbox/Drafts/autoexpunge=', p.retention_continuity, "d\t",       \
       'namespace/inbox/mailbox/Quarantine/autoexpunge=', p.retention_continuity, "d\t",   \
       'namespace/inbox/mailbox/Quarantine.Spam/autoexpunge=', p.retention_continuity, "d\t", \
       'namespace/inbox/mailbox/Quarantine.BadHeaders/autoexpunge=', p.retention_continuity, "d\t", \
       'namespace/inbox/mailbox/Quarantine.BannedFiles/autoexpunge=', p.retention_continuity, "d\t", \
       'namespace/inbox/mailbox/Quarantine.Blocked/autoexpunge=', p.retention_continuity, "d\t", \
       'namespace/inbox/mailbox/Quarantine.Virus/autoexpunge=', p.retention_continuity, "d\t", \
       'namespace/inbox/mailbox/Trash/autoexpunge=', p.retention_continuity, "d\t",        \
       'namespace/inbox/mailbox/Sent/autoexpunge=', p.retention_continuity, "d"), '')      \
as userdb_import
  FROM auth_user u LEFT JOIN amavisd_emailaccount e ON u.id=e.user_id \
    LEFT JOIN amavisd_localpartalias la ON e.id=la.email_account_id \
    LEFT JOIN amavisd_domainalias da ON la.domain_id=da.domain_id  \
    LEFT JOIN amavisd_domain d ON la.domain_id=d.id  \
    LEFT JOIN amavisd_policy p ON d.policy_id=p.id \
  WHERE (la.localpart='%n' OR la.localpart = substring_index('%n','-', 1) OR la.localpart = substring_index('%n','+', 1)  ) and (da.name = '%d' OR la.external_domain = '%d')  and (p.continuity_enabled=1)) dt
