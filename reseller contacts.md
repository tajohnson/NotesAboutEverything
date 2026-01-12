reseller contacts

select r.name,c.first_name, c.last_name, c.email, c.phone from amavisd_reseller as r left join amavisd_contact_reseller as cr on r.id=cr.related_object_id left join amavisd_contact as c on cr.contact_ptr_id=c.id where contact_ptr_id is not null;




select distinct(c.email) as technical from amavisd_reseller as r left join amavisd_contact_reseller as cr on r.id=cr.related_object_id left join amavisd_contact as c on cr.contact_ptr_id=c.id where contact_ptr_id is not null and c.is_technical=1;
select distinct(c.email) as billing from amavisd_reseller as r left join amavisd_contact_reseller as cr on r.id=cr.related_object_id left join amavisd_contact as c on cr.contact_ptr_id=c.id where contact_ptr_id is not null and c.is_billing=1;
select distinct(c.email) as emergency from amavisd_reseller as r left join amavisd_contact_reseller as cr on r.id=cr.related_object_id left join amavisd_contact as c on cr.contact_ptr_id=c.id where contact_ptr_id is not null and c.is_emergency=1;



List contacts of direct customers.

select distinct(c.email) as technical from amavisd_customer as r left join amavisd_contact_customer as cr on r.id=cr.related_object_id left join amavisd_contact as c on cr.contact_ptr_id=c.id where contact_ptr_id is not null and c.is_technical=1 and reseller_id=1;
select distinct(c.email) as billing from amavisd_customer as r left join amavisd_contact_customer as cr on r.id=cr.related_object_id left join amavisd_contact as c on cr.contact_ptr_id=c.id where contact_ptr_id is not null and c.is_billing=1 and reseller_id=1;;
select distinct(c.email) as emergency from amavisd_customer as r left join amavisd_contact_customer as cr on r.id=cr.related_object_id left join amavisd_contact as c on cr.contact_ptr_id=c.id where contact_ptr_id is not null and c.is_emergency=1 and reseller_id=1;;



List contacts of MRCS customers.

select distinct(c.email) as technical from amavisd_customer as r left join amavisd_contact_customer as cr on r.id=cr.related_object_id left join amavisd_contact as c on cr.contact_ptr_id=c.id where contact_ptr_id is not null and c.is_technical=1 and reseller_id=154;
select distinct(c.email) as billing from amavisd_customer as r left join amavisd_contact_customer as cr on r.id=cr.related_object_id left join amavisd_contact as c on cr.contact_ptr_id=c.id where contact_ptr_id is not null and c.is_billing=1 and reseller_id=154;;
select distinct(c.email) as emergency from amavisd_customer as r left join amavisd_contact_customer as cr on r.id=cr.related_object_id left join amavisd_contact as c on cr.contact_ptr_id=c.id where contact_ptr_id is not null and c.is_emergency=1 and reseller_id=154;;





# technical contacts of fastmail domains

mysql> select c.email as technical,r.name,d.name from amavisd_customer as r left join amavisd_domain as d on r.id=d.customer_id left join amavisd_mailserver s on d.id=s.domain_id left join amavisd_contact_customer as cr on r.id=cr.related_object_id left join amavisd_contact as c on cr.contact_ptr_id=c.id where contact_ptr_id is not null and c.is_technical=1 and s.server like "%messagingengine.com";