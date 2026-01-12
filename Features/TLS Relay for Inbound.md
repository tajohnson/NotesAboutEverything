Use /etc/postfix/tls_relay.in (or a mysql map that retrieves the value from the amavisd_domain table) to determine how they want to do it.  tls_relay.in is in the format of "nexthop:port <value>", like "terramar.net.ms.mailroute.net:25	none"
      Options:
         none
         may ("Opportunistic")
         encrypt ("Force/Mandatory Encryption")
         ??? verify ("mandatory server certificate verification" - remote SMTP server certificate can be validated (not expired or revoked, and signed by a trusted Certification Authority)
   	
   	mysql query might be something like this:
   		select concat(da.name, ".ms.mailroute.net:", deliveryport, "<tab>", d.tls_policy ) as result  
   		  from amavisd_domainalias as da 
   		  left join amavisd_domain as d on d.id=da.domain_id 
   		  left join amavisd_mailserver as s on s.domain_id=d.id 
   		  where da.name = '%d';