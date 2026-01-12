cd /var/named/chroot/var/named/zones/master

Create a Zone Signing Key(ZSK) with the following command

	dnssec-keygen -a NSEC3RSASHA1 -b 2048 -n ZONE example.com

Create a Key Signing Key(KSK) with the following command

	dnssec-keygen -f KSK -a NSEC3RSASHA1 -b 4096 -n ZONE example.com

Add these keys to the zone file:
	for key in `ls Kexample.com*.key`
	do
	echo "\$INCLUDE $key">> example.com.zone
	done

	
Sign the zone with a random salt:
	dnssec-signzone -A -3 $(head -c 1000 /dev/random | sha1sum | cut -b 1-16) -N INCREMENT -o example.com -t example.com.zone
	
	
Update bind to point at new signed zone file:

vi /var/named/chroot/var/named/named.zones.conf
zone "terramar.net" {
      type master;
      file "zones/master/terramar.net.signed";
    
 };
	
	
reload named
	service named reload
	
	
Check it:

	dig DNSKEY mailroute.net. @localhost +multiline
	
Check for RRSIG:

	dig A mailroute.net. @localhost +noadditional +dnssec +multiline
	
	
Check to make sure slaves load signed files


Configure DS records with the registrar

When we ran the dnssec-signzone command apart from the .signed zone file, a file named dsset-
example.com was also created, this contains the DS records.

root@master:/var/cache/bind# cat dsset-example.com.
example.com.        IN DS 62910 7 1 1D6AC75083F3CEC31861993E325E0EEC7E97D1DD
example.com.        IN DS 62910 7 2 198303E265A856DE8FE6330EDB5AA76F3537C10783151AEF3577859F FFC3F59D

These have to be entered in your domain registrar's control panel.




Modifying Zone Records
Each time you edit the zone by adding or removing records, it has to be signed to make it work. So we will create a script for this so that we don't have to type long commands every time.
root@master# nano /usr/sbin/zonesigner.sh

#!/bin/sh
PDIR=`pwd`
ZONEDIR="/var/cache/bind" #location of your zone files
ZONE=$1
ZONEFILE=$2
DNSSERVICE="bind9" #On CentOS/Fedora replace this with "named"
cd $ZONEDIR
SERIAL=`/usr/sbin/named-checkzone $ZONE $ZONEFILE | egrep -ho '[0-9]{10}'`
sed -i 's/'$SERIAL'/'$(($SERIAL+1))'/' $ZONEFILE
/usr/sbin/dnssec-signzone -A -3 $(head -c 1000 /dev/random | sha1sum | cut -b 1-16) -N increment -o $1 -t $2
service $DNSSERVICE reload
cd $PDIR

	
Run it like this:
	root@master# zonesigner.sh mailroute.net mailroute.net
	
	
	
	
Add a cron job to make this happen every few days, to prevent zone walking from somebody breaking the salt

0       0       */3     *       *       /usr/sbin/zonesigner.sh mailroute.net mailroute.net
	
	