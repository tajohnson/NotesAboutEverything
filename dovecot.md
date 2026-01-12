

sudo -u mailowner /opt/dovecot-2.0.15-lmtpproxytimeout180/bin/doveadm mailbox status -A vsize '*'



sudo -u mailowner /opt/dovecot-2.0.15-lmtpproxytimeout180/bin/doveadm force-resync 'tj@terramar.net'


test.sh:

#/bin/bash

STR=tj@mailroute.net

echo $STR
#echo ${STR:0:2}

LOCALPART="${STR%@*}"
DOMAIN="${STR#*@}"

echo "/data/mail/${DOMAIN:0:1}/${DOMAIN:1:1}/${DOMAIN}/${LOCALPART:0:1}/${LOCALPART:1:1}/${LOCALPART}"


list all users

select distinct(concat(e.localpart, '@', d.name))  from auth_user u    left join amavisd_emailaccount e on u.id=e.user_id    left join amavisd_localpartalias la on e.id=la.email_account_id    left join amavisd_domainalias d on la.domain_id=d.domain_id   left join temporary_hosted h on d.id=h.domain_id where d.name = '$domain' AND (h.continuity=1 OR h.hosted=1);


select distinct(concat(e.localpart, '@', d.name))  from auth_user u    left join amavisd_emailaccount e on u.id=e.user_id    left join amavisd_localpartalias la on e.id=la.email_account_id    left join amavisd_domainalias d on la.domain_id=d.domain_id   left join temporary_hosted h on d.id=h.domain_id where d.name = '$domain' AND (h.continuity=1 OR h.hosted=1);

concat('/data/mail/', substring(name,1,1), '/', substring(name,2,1), '/', name)