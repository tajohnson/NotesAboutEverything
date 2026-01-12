IPMI
	login: admin
	password: <see "Secure Info">
	
	login: tjohnson
	- [ ] password: <see "Secure Info">
	
	
	login: monitor
	password: <see "Secure Info">
	priv: user
	
	
	IPMI configuration
	SMTP
		server: 64.237.50.187
		port: 587
		Username: ipmi@mailroute.net
		password: <see "Secure Info">
		Sender Address: ipmi@mailroute.net
		
	login: admin
	password: <see "Secure Info">
	
	login: tjohnson
	password: <see "Secure Info">
	
	
	login: monitor
	password: <see "Secure Info">
	priv: user
	
	Alert
		Alert level - info
		email: tj@mailroute.net
		Subject: IPMI - Alert
	SMTP
		SSL Auth unchecked
		SMTP Server: 64.237.50.187
		Port: 25
		Username:
		Password:
		Sender's address: ipmi@mailroute.net
	Date & Time:
		tick.ucla.edu
		tock.ucla.edu
	

ipmi license generator:  (use ipmi mac address)
 echo -n 'ae:1f:6b:0b:3d:72' | xxd -r -p | openssl dgst -sha1 -mac HMAC -macopt hexkey:8544E3B47ECA58F9583043F8 | awk '{print $2}' | cut -c 1-24



005.mia 

	ipmi |IP Address: 199.089.003.136|
	BMC MAC Address: ae:1f:6b:0b:3d:72|
	LAN1 MAC Address: ac:1f:6b:c7:9e:8a|
	LAN2 MAC Address: ac:1f:6b:c7:9e:8b|
	LAN3 MAC Address: ac:1f:6b:c7:9e:8c|
	LAN4 MAC Address: ac:1f:6b:c7:9e:8d|

	License key: 2201 4aea 97d8 ec5e 4971 9d88


006.mia ipmi mac:

	ipmi IP Address: 199.089.003.137
	BMC MAC Address: ae:1f:6b:0b:41:7e
	LAN1 MAC Address: ac:1f:6b:0b:41:7e
	LAN2 MAC Address: ac:1f:6b:0b:41:7f
	LAN3 MAC Address: ac:1f:6b:0b:41:80
	LAN4 MAC Address: ac:1f:6b:0b:41:81


es01.mia ipmi mac:
	ipmi IP Address: 199.089.003.181|
	BMC MAC Address: ae:1f:6b:c5:31:9e|
	LAN1 MAC Address: ac:1f:6b:c5:31:9e|
	LAN2 MAC Address: ac:1f:6b:c5:31:9f|
	LAN3 MAC Address: ac:1f:6b:c5:31:a0|
	LAN4 MAC Address: ac:1f:6b:c5:31:a1|



es02.mia ipmi-mac
	ipmi mac: 0e:c4:7a:87:05:34
	
	LAN1 MAC Address: 0c:c4:7a:87:05:34|
	LAN2 MAC Address: 0c:c4:7a:87:05:35|
	LAN3 MAC Address: 0c:c4:7a:87:05:36|
	LAN4 MAC Address: 0c:c4:7a:87:05:37|




es01 and es02 - each has 2 7.5tb nvme drives + the 4TB boot drive

Mount the 13.8TB striped volume on /data



wipefs --all /dev/nvme1n1 /dev/nvme2n1
pvcreate /dev/nvme1n1 /dev/nvme2n1
vgcreate data_vg /dev/nvme1n1 /dev/nvme2n1
lvcreate -i 2 -I 64 -L 13.8T -n data_lv data_vg
mkfs.xfs -L data /dev/data_vg/data_lv
mkdir -p /data
mount -t xfs -o rw,noatime,nodiratime,attr2,noquota /dev/data_vg/data_lv /data

df -hT /data

add to /etc/fstab:
/dev/data_vg/data_lv /data xfs defaults,rw,noatime,nodiratime,attr2,noquota 0 0






Power utilization - 2025-11-13  (min, avg, peak)
		001.mia 126 149 300W
		002.mia 106 158 365
		003.mia 121 156 362
		004.mia  116 174 385
		es01.mia 71 119 406
		es02.mia  71 119 461
		switches x3 @ 80w each = 240

Total (average): 875 Watts + 240W = 1115 W.
20A/120V = 1920 Watts max - could double serves without a power upgrade!





