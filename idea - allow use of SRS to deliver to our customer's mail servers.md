
For those who cannot disable SPF checks.

Idea: add a "use srs to relay customer mail" type of flag to amavisd_domain table.
on posfix inbound, modify the mysql lookup to return "relay:[forward.mailroute.net]:2525" for those domains, which will send the email to a postfix-forward instance.

on postfix-forward - add a transport map for the customer, and return 
```
relay:[theiremailserver.com]:25
```
so that there's no mail loop.


or maybe the inbound server for domain.com.ms.mailroute.net is set to forward.mailroute.net?  Then no lookup change required on postfix-inbound transport.mysql map.

But need do need to have the forwarding mailserver set somewhere else in the interface, if using SRS for all mail relay.  Where would that go from a UI standpoint?


