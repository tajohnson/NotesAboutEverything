
# This will find all combinations of localpartalias cross join domainalias + external aliases

select distinct concat(a, '@', b) as email from (select distinct la.localpart as a from amavisd_localpartalias la left join amavisd_localpartalias la2 on la.email_account_id=la2.email_account_id where la2.localpart='tj' and la2.domain_id IN (select domain_id from amavisd_domainalias where name = 'terramar.net')) as  t1 cross join (select distinct da.name as b from amavisd_domainalias da left join amavisd_domainalias da2 on da.domain_id=da2.domain_id where da2.name = 'terramar.net') as t2 union select concat(lae.localpart, '@', lae.external_domain) from amavisd_localpartalias lae WHERE lae.external_domain is not null AND lae.localpart='tj' and lae.domain_id IN (select domain_id from amavisd_domainalias where name = 'terramar.net')


# test for an acm user:

select distinct concat(a, '@', b) as email from 
(select distinct la.localpart as a from amavisd_localpartalias la left join amavisd_localpartalias la2 on la.email_account_id=la2.email_account_id where la2.localpart='greenberg' and la2.domain_id IN (select domain_id from amavisd_domainalias where name = 'hq.acm.org')) as  t1 cross join
(select distinct da.name as b from amavisd_domainalias da left join amavisd_domainalias da2 on da.domain_id=da2.domain_id where da2.name = 'hq.acm.org') as t2
union
select concat(lae.localpart, '@', lae.external_domain) from amavisd_localpartalias lae WHERE lae.external_domain is not null AND lae.localpart='greenberg' and lae.domain_id IN (select domain_id from amavisd_domainalias where name = 'hq.acm.org')
