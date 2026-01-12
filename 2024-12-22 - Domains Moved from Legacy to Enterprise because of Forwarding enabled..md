
Should maybe have a legacy+forwarding plan for these customers instead of enterprise



```


All ACM Domains:



mysql> select distinct d.id,d.name from forwarding_forwardingemail f left join amavisd_emailaccount e on f.email_account_id=e.id left join amavisd_domain d on d.id=e.domain_id where customer_id != 3858;
+-------+------------------------------+
| id    | name                         |
+-------+------------------------------+
|   988 | islandemail.com              |
|   990 | islandtechnologies.net       |
|  1899 | terramar.net                 |
|  5049 | essai.com                    |
|  5167 | ela.com                      |
|  6962 | newlisbon.k12.wi.us          |
| 10329 | biomni.com                   |
| 11258 | hilltopranch.com             |
| 13239 | kerecis.is                   |
| 13414 | schwartzmeyer.com            |
| 13594 | anmartinsystems.com          |
|  3628 | sassecurity.com              |
| 14842 | mgeinc.com                   |
| 14918 | goelitenp.com                |
| 14951 | us.kerecis.com               |
| 10750 | envision-ink.com             |
| 15398 | mailroutedemo.com            |
| 15000 | int.kerecis.com              |
| 15624 | 911petchip.com               |
| 15142 | branament.com                |
| 15658 | islay.islandtechnologies.net |
| 13762 | felixlighting.com            |
|  3610 | scenicwonders.com            |
| 15775 | ispsupplies.com              |
| 15787 | rvctexas.com                 |
| 15877 | ragosta.com                  |
| 16319 | corelis.com                  |
| 16486 | elainc.net                   |
| 16489 | corp.kerecis.com             |
+-------+------------------------------+
29 rows in set (0.71 sec)

mysql>

```
