select concat("update add ", d.name, ".ms.mailroute.net 60 mx ", s.priority, " ", s.server )
from amavisd_domain d left join amavisd_mailserver s on d.id=s.domain_id where d.id in 
(select d.id  from amavisd_domain d left join amavisd_customer c on d.customer_id=c.id left join amavisd_reseller r on r.id=c.reseller_id where c.reseller_id=380);



