It's up and running on lax-fs01.


Key signing has been added.  Keys are in lax-fs01:/usr/local/etc/named/


make sure nobody outside 199.89.0.0/21 can query
make sure fs01 is transferring to slaves (everything else)
what about a redundant master? Is that possible?

The blank line is IMPORTANT!

nsupdate -k /usr/local/etc/named/keys/Kmailroute.net.+157+16972.private
> server 127.0.0.1
> update add mailroute.net.ms.mailroute.net 20 mx 10 test01.mailroute.net.
> 
> 

update add domain.com.ms.mailroute.net. (TTL) (CLASS) (MX PRIORITY) (SERVER.)

 nsupdate  
> server 199.89.0.102
> update add test3.ms.mailroute.net. 30 mx 10 test01.mailroute.net.
> update add test3.ms.mailroute.net. 30 mx 10 test02.mailroute.net.
> update add test3.ms.mailroute.net. 30 mx 20 test03.mailroute.net.
> 
> [root@lax-gw02 zones]# rndc freeze ms.mailroute.net 
[root@lax-gw02 zones]# cat ms.mailroute.net 
$ORIGIN .
$TTL 600	; 10 minutes
ms.mailroute.net	IN SOA	ns02.mailroute.net. tj.mailroute.net. (
				2008013005 ; serial
				7200       ; refresh (2 hours)
				1200       ; retry (20 minutes)
				2419200    ; expire (4 weeks)
				3600       ; minimum (1 hour)
				)
			NS	ns03.mailroute.net.
			NS	ns02.mailroute.net.
			MX	10 mail.mailroute.net.
$ORIGIN ms.mailroute.net.
localhost		A	127.0.0.1
test			MX	10 mail01.mailroute.net.
			MX	10 mail02.mailroute.net.
			MX	20 mail03.mailroute.net.
$TTL 20	; 20 seconds
test2			MX	10 mail01.mailroute.net.
			MX	20 mail02.mailroute.net.
$TTL 30	; 30 seconds
test3			MX	10 test01.mailroute.net.
			MX	10 test02.mailroute.net.
			MX	20 test03.mailroute.net.
[root@lax-gw02 zones]# rndc unfreeze ms.mailroute.net
[root@lax-gw02 zones]# 
