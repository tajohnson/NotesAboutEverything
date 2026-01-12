
- [x] Islandemail domains on hold at 6am

```
SELECT d.name, d.hold_email ,m.server FROM  amavisd_domain d
JOIN amavisd_customer c ON d.customer_id = c.id
JOIN amavisd_mailserver m ON m.domain_id = d.id
WHERE c.reseller_id = 2
AND m.server = 'mail.islandemail.com';

UPDATE amavisd_domain d
JOIN amavisd_customer c ON d.customer_id = c.id
JOIN amavisd_mailserver m ON m.domain_id = d.id
SET d.hold_email = 1
WHERE c.reseller_id = 2
AND m.server = 'mail.islandemail.com';


Reset it after:

UPDATE amavisd_domain d
JOIN amavisd_customer c ON d.customer_id = c.id
JOIN amavisd_mailserver m ON m.domain_id = d.id
SET d.hold_email = 0
WHERE c.reseller_id = 2
AND m.server = 'mail.islandemail.com';



```

- [x] Release islandemail hold when patrick requests, after move.


Tickets - requests to psychz to advertise routes
  ## 4529266


- [ ] Need to set up 008.lax from scratch (was dbq01)  need to set up anew