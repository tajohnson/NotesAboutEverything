Would be nice to know when people are recovering emails from blacklisted senders, wouldn't it?


mysql> select d.name,m.email,wb.wb from amavisd_wblist wb left join amavisd_mailaddr m on m.id=wb.sid left join amavisd_domain d on d.uuid=wb.rid  where m.email in (select email from freemail) AND wb.wb in ('w', 'W') order by wb.wb;



select concat(e.localpart, '@', d.name),count(*) as blacklisted from amavisd_wblist wb left join amavisd_emailaccount e on wb.rid=e.uuid left join amavisd_domain d on e.domain_id=d.id  where wb.wb in ('B','b') group by wb.rid order by blacklisted DESC;


select d.name,count(*) as blacklisted from amavisd_wblist wb left join amavisd_domain d on wb.rid=d.uuid where wb.wb in ('B','b') group by wb.rid order by blacklisted DESC; 



list users and domains with how many whitelists they have:

select concat(e.localpart, '@', d.name),count(*) as whitelisted from amavisd_wblist wb left join amavisd_emailaccount e on wb.rid=e.uuid left join amavisd_domain d on e.domain_id=d.id  where wb.wb in ('W','w') group by wb.rid order by whitelisted DESC; select d.name,count(*) as whitelisted from amavisd_wblist wb left join amavisd_domain d on wb.rid=d.uuid where wb.wb in ('W','w') group by wb.rid order by whitelisted DESC; 