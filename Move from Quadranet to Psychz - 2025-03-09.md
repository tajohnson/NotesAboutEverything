


- [ ] Put email on hold for Island.


```
UPDATE amavisd_domain d
JOIN amavisd_customer c ON d.customer_id = c.id
JOIN amavisd_mailserver m ON m.domain_id = d.id
SET d.hold_email = 1
WHERE c.reseller_id = 2
AND m.server = 'mail.islandemail.com';

select d.name,d.hold_email,m.server from  amavisd_domain d
JOIN amavisd_customer c ON d.customer_id = c.id
JOIN amavisd_mailserver m ON m.domain_id = d.id
WHERE c.reseller_id = 2
AND m.server = 'mail.islandemail.com';


reset it afterwards:

UPDATE amavisd_domain d
JOIN amavisd_customer c ON d.customer_id = c.id
JOIN amavisd_mailserver m ON m.domain_id = d.id
SET d.hold_email = 0
WHERE c.reseller_id = 2
AND m.server = 'mail.islandemail.com';




