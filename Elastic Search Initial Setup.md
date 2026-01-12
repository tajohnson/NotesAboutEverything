Elastic Search Initial Setup
:
  do ansible-playbook ./playbooks/search.yml --limit es03.lax.int.mailroute.net
  
  be sure to update all server to latest ES - before adding es03 to cluster
  
  



New ES cluster in Los Angeles.

5 servers - 4 data and 1 voting-only master offsite  to break ties

Current usage is about 3.5TB (including replicas).  New configuration has 4 servers each with 4 3.84 tb storage.  Using JBOD, so that's 61.4TB of storage.  Should be fine for a while.  We could grow 20-fold or increase number of replicas and not worry.


ES user: controlpanel (used for maillogs, quarantine, etc)
password: ciwxaf-wawpoT-5hevhe

rack i3:
	es01
	es03
rack j4:
	es02
	es04
	
	
Ask Met

Quadranet (Mailroute CID 5892)
530 West 6th Street, #901
Los Angeles, CA 90014



	
Met to label servers, front and back
	es01-lax
	es02-lax
	es03-lax
	es04-lax
	
Please be sure the M.2 drive is connected the PCI-E socket of the riser card, not the SATA socket.
	
Met-servers will configure IPMI

	es01-lax: 199.89.1.181
	es02-lax: 199.89.1.182
	es03-lax:	199.89.1.183
	es04-lax:	199.89.1.184
	
	Subnet mask: 255.255.255.0
	Gateway: 199.89.1.1
	DNS: 199.89.1.4
	
	User with Administrator privileges:  tempuser
	password:  tempuser98123
	
Each server configured as follows:
	boot: Intel SSDPEKKF010T8 x 1 (Intel SSDPEKKF010T8 1.0TB M.2)
	data: Samsung MZQLB3T8HALS x 4 (Samsung MZQLB3T8HALS-00007 3.84TB PM983 Pci-E 3.0 X4 2.5inch NVMe SSD)

	Supermicro SuperServer SYS-6019U- TN4R4T 4 Bay LFF 1U NVMe Server Processor: 	2x Intel Xeon Gold 6148 2.4GHz 20 Core 24.75MB Cache 150W Processor
	Memory: 256GB DDR4 ECC Registered Memory (16 x 16GB)
	Backplane: 4-port 1U SAS3 12Gbps Hybrid Backplane BPN-SAS3-815TQ-N4
	RAID Controller: Onboard SATA RAID Controller (0 1 5 10)
	NVMe Hard Drive Bay: 4x 3.84TB U.2 PCIe NVMe 2.5 Solid State Drive
	M.2 Storage: 1TB M.2 NVME SSD Management: Intelligent Platform Management 	Interface 2.0
	Onboard Networking: 4x 10Gbase-T via AOC-UR-i4XT
	Power Supplies: 2x 750W Redundant Power Supplies
	Rail Kit: Supermicro Original Rail Kit Included
	Extended Warranty: RackGuard Standard - 90 Days (Replacement Parts No Onsite Support)
	
	

Ask Quadranet to install servers as follows:

	First, please unplug the juniper switches in both cabinets.  Remove if space is needed.
	
	Next, please install two of these 1U servers in each of the two racks:

	es01.lax - rack i3
	es02.lax - rack j4
	es03.lax - rack i3
	es04.lax - rack j4
	
	The servers each have dual power supplies.  Each rack has at least two PDUs.  Please use different PDUs for each of the different power supplies of each server.
	
	
	Please connect the ethernet of each server to the two different switches in the rack
	   The LAN ports on the back are arranged:
	   	3  4
			1  2
		
		Please plug cables from each of LAN port 1 and 2 into the HP switch labeled pr01 or pr02 switch in the rack.  Please use one color for Port 1 and a different for Port 2, if possible (if not, that's okay)
		
		Please plug a cable from the IPMI port to the HP switch labled ipmi01 or ipmi02 in the rack
		
		
		
Plan: 2 new switches for private network: Aruba 1960 JL808A
	Connect ES servers to the 10Gb-T ports
	Use optical to cross-connect the two switches in the two racks
	
	Leave the public network on the existing PR01/PR02 switches.  Leave IPMI on those switches too, or on existing IPMI switches
	
	
	
	
	Public Network: HP Procurve Swit
		
		
IPMI configuration
	SMTP
		server: 64.237.50.187
		port: 587
		Username: 
		password: 
		Sender Address: ipmi@mailroute.net
		

	
	

	
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
		
es01
	Firmware Revision: 01.74.11
	IP Address: 199.089.001.181
	Firmware Build Time: 10/27/2022
	BMC MAC Address: ae:1f:6b:c7:9e:a2
	BIOS Version: 3.8a
	System LAN1 MAC Address: ac:1f:6b:c7:9e:a2
	BIOS Build Time: 10/28/2022
	System LAN2 MAC Address: ac:1f:6b:c7:9e:a3
	Redfish Version: 1.8.0
	System LAN3 MAC Address: ac:1f:6b:c7:9e:a4
	CPLD Version: 03.B0.06
	System LAN4 MAC Address: ac:1f:6b:c7:9e:a5
	
	IPv6 Address1 : fe80:0:0:0:ac1f:6bff:fec7:9ea2/64

es02
	Firmware Revision: 01.74.11
	IP Address: 199.089.001.182
	Firmware Build Time: 10/27/2022
	BMC MAC Address: ae:1f:6b:0b:3c:22
	BIOS Version: 3.8a
	System LAN1 MAC Address: ac:1f:6b:0b:3c:22
	BIOS Build Time: 10/28/2022
	System LAN2 MAC Address: ac:1f:6b:0b:3c:23
	Redfish Version: 1.8.0
	System LAN3 MAC Address: ac:1f:6b:0b:3c:24
	CPLD Version: 03.B0.06
	System LAN4 MAC Address: ac:1f:6b:0b:3c:25
	
	IPv6 Address1 : fe80:0:0:0:ac1f:6bff:fe0b:3c22/64
		
	
es03
	Firmware Revision: 01.74.02
	IP Address: 199.089.001.183
	Firmware Build Time: 07/07/2022
	BMC MAC Address: ae:1f:6b:0b:40:06
	BIOS Version: 3.8a
	System LAN1 MAC Address: ac:1f:6b:0b:40:06
	BIOS Build Time: 10/28/2022
	System LAN2 MAC Address: ac:1f:6b:0b:40:07
	Redfish Version: 1.8.0
	System LAN3 MAC Address: ac:1f:6b:0b:40:08
	CPLD Version: 03.B0.06
	System LAN4 MAC Address: ac:1f:6b:0b:40:09
	
	IPv6 Address1 : fe80:0:0:0:ac1f:6bff:fe0b:4006/64



es04
	Firmware Revision: 01.74.11
	IP Address: 199.089.001.184
	Firmware Build Time: 10/27/2022
	BMC MAC Address: ae:1f:6b:0a:a0:0a
	BIOS Version: 3.8a
	System LAN1 MAC Address: ac:1f:6b:0a:a0:0a
	BIOS Build Time: 10/28/2022
	System LAN2 MAC Address: ac:1f:6b:0a:a0:0b
	Redfish Version: 1.8.0
	System LAN3 MAC Address: ac:1f:6b:0a:a0:0c
	CPLD Version: 03.B0.06
	System LAN4 MAC Address: ac:1f:6b:0a:a0:0d
	
	IPv6 Address1 : fe80:0:0:0:ac1f:6bff:fe0a:a00a/64


Public Network
	es01	199.89.1.80
	es02	199.89.1.81
	es03	199.89.1.82
	es04	199.89.1.83
	
Private Network
	es01
		172.16.145.231 255.255.255.192 172.16.145.193
		dns: 172.16.145.204 172.16.145.205
	es02
		172.16.145.232 255.255.255.192 172.16.145.193
		dns: 172.16.145.204 172.16.145.205
	es03
		172.16.145.233 255.255.255.192 172.16.145.193
		dns: 172.16.145.204 172.16.145.205
	es04
		172.16.145.234 255.255.255.192 172.16.145.193
		dns: 172.16.145.204 172.16.145.205

		
Private network routes
	172.16.0.0 255.240.0.0 172.16.145.93
	


ES Setup plan.

New servers are running Alma Linux 9.  So we need a new ansible master.  That will be es01 for now.  Eventually, we migrate all other servers to Alma 9, and move the ansible master over somewhere else.

Let's use es01 as the initial ansible master/software building server.

Need to see what needs to be done from the old "setup.yml" for these new servers and start creating new ansible roles for each thing.





Disk setup - 4x4tb drives

# if needed:    sgdisk --zap-all /dev/nvme1n1
pvcreate -f /dev/nvme1n1 /dev/nvme2n1 /dev/nvme3n1 /dev/nvme4n1
vgcreate es_group /dev/nvme1n1 /dev/nvme2n1 /dev/nvme3n1 /dev/nvme4n1
lvcreate -i 4 -I 4 -L 13.8T -n es_data es_group
mkfs.xfs -L es01 /dev/es_group/es_data
mount -t xfs -o rw,noatime,nodiratime,attr2,noquota /dev/es_group/es_data /var/data/elasticsearch


Adding a node

on es01: 
   ans
   ansible-playbook ./playbooks/search.yml --limit newhost.lax.mailroute.net
   
on host
	just start it
	
	
	
	
  
Create a file-based superuser, in case things go south on restore:

./bin/elasticsearch-users useradd tjsuper -p man0uver -r superuser



  
Set up old s3 repo and make it read-only

User: mr-es-snapshot
	Access Key: See "Secure Info"
	Secret Key: See "Secure Info"


2024-10-10 - works on cluster :)

# curl --cacert /etc/elasticsearch/certs/elastic-stack-ca.pem --user 'elastic:<password>' 'https://es01.lax.int.mailroute.net:9200/_cat/indices?v'


curl --cacert /etc/elasticsearch/certs/elastic-stack-ca.pem --user 'elastic:<password>'  -XDELETE https://es01.lax.int.mailroute.net:9200/logstash-2016.05.31





curl --cacert /etc/elasticsearch/certs/elastic-stack-ca.pem --user 'elastic:<password>' -X GET "https://es01.lax.int.mailroute.net:9200/_cat/repositories?v"


curl  --cacert /etc/elasticsearch/certs/elastic-stack-ca.pem  --user 'elastic:<password>' -X PUT "https://es01.lax.int.mailroute.net:9200/_snapshot/mr-es-snapshot-lax?pretty" -H 'Content-Type: application/json' -d'
{
  "type": "s3",
  "settings": {
    "endpoint": "s3.us-west-1.amazonaws.com",
    "bucket": "mr-es-snapshot-lax",
    "readonly": true
  }
}
'



Restoring entire cluster - wonder if this will work when moving from 7 to 8

Do this on NEW cluster:


	to monitor it while it's running:
		curl --cacert /etc/elasticsearch/certs/elastic-stack-ca.pem --user 'elastic:<password>'  -X GET "https://es01.lax.int.mailroute.net:9200/_cluster/health?pretty"


curl --cacert /etc/elasticsearch/certs/elastic-stack-ca.pem --user 'elastic:<password>'  -X PUT "https://es01.lax.int.mailroute.net:9200/_cluster/settings?pretty" -H 'Content-Type: application/json' -d'{"persistent": {"ingest.geoip.downloader.enabled": false,"indices.lifecycle.history_index_enabled": false}}'
curl --cacert /etc/elasticsearch/certs/elastic-stack-ca.pem --user 'elastic:<password>'  -X POST "https://es01.lax.int.mailroute.net:9200/_ilm/stop?pretty"
curl --cacert /etc/elasticsearch/certs/elastic-stack-ca.pem --user 'elastic:<password>'  -X POST "https://es01.lax.int.mailroute.net:9200/_ml/set_upgrade_mode?enabled=true&pretty"
curl --cacert /etc/elasticsearch/certs/elastic-stack-ca.pem --user 'elastic:<password>'  -X PUT "https://es01.lax.int.mailroute.net:9200/_cluster/settings?pretty" -H 'Content-Type: application/json' -d'{"persistent": {"xpack.monitoring.collection.enabled": false}}'
curl --cacert /etc/elasticsearch/certs/elastic-stack-ca.pem --user 'elastic:<password>'  -X POST "https://es01.lax.int.mailroute.net:9200/_watcher/_stop?pretty"
curl --cacert /etc/elasticsearch/certs/elastic-stack-ca.pem --user 'elastic:<password>'  -X PUT "https://es01.lax.int.mailroute.net:9200/_cluster/settings?pretty" -H 'Content-Type: application/json' -d'{"persistent": {"action.destructive_requires_name": false}}'

curl --cacert /etc/elasticsearch/certs/elastic-stack-ca.pem --user 'elastic:<password>'  -X DELETE "https://es01.lax.int.mailroute.net:9200/_data_stream/*?expand_wildcards=all&pretty"

curl --cacert /etc/elasticsearch/certs/elastic-stack-ca.pem --user 'elastic:<password>'  -X DELETE "https://es01.lax.int.mailroute.net:9200/*?expand_wildcards=all&pretty"



Now restore from the snapshot:

curl --cacert /etc/elasticsearch/certs/elastic-stack-ca.pem --user 'elastic:<password>'  -X POST "https://es01.lax.int.mailroute.net:9200/_snapshot/mr-es-snapshot-lax/lax-daily-snapshot-2023.05.05-8jl69r5asoczzs2yfpde8q/_restore?pretty" -H 'Content-Type: application/json' -d'{"indices": "*-amavisd,*-policyd,*-postfix,tmp-*,report*"},"include_global_state": true}'

	to monitor it while it's running:
		https://newkibana.mailroute.net/app/management/data/snapshot_restore/restore_status


Once restore is finished, re-enable everything on new cluster:

curl --cacert /etc/elasticsearch/certs/elastic-stack-ca.pem --user 'elastic:<password>'  -X PUT "https://es01.lax.int.mailroute.net:9200/_cluster/settings?pretty" -H 'Content-Type: application/json' -d'{"persistent": {"ingest.geoip.downloader.enabled": true,"indices.lifecycle.history_index_enabled": true}}'
curl --cacert /etc/elasticsearch/certs/elastic-stack-ca.pem --user 'elastic:<password>'  -X POST "https://es01.lax.int.mailroute.net:9200/_ilm/start?pretty"
curl --cacert /etc/elasticsearch/certs/elastic-stack-ca.pem --user 'elastic:<password>'  -X POST "https://es01.lax.int.mailroute.net:9200/_ml/set_upgrade_mode?enabled=false&pretty"
curl --cacert /etc/elasticsearch/certs/elastic-stack-ca.pem --user 'elastic:<password>'  -X PUT "https://es01.lax.int.mailroute.net:9200/_cluster/settings?pretty" -H 'Content-Type: application/json' -d'{"persistent": {"xpack.monitoring.collection.enabled": true}}'
curl --cacert /etc/elasticsearch/certs/elastic-stack-ca.pem --user 'elastic:<password>'  -X POST "https://es01.lax.int.mailroute.net:9200/_watcher/_start?pretty"



after generating http key, add the other key to it:
keytool -importkeystore -destkeystore http.p12 -srckeystore elastic-stack-ca.p12 -srcstoretype PKCS12

convert ca p12 file to a pem:
openssl pkcs12 -in /etc/elasticsearch/certs/elastic-stack-ca.p12 -out /etc/elasticsearch/certs/elastic-stack-ca.pem -clcerts -nokeys

Passwords:
	password for ca: <See "Secure Info">
	password for node certs: <See "Secure Info">
	password for http cert: <See "Secure Info">
	
	
	kibana_system password: <See "Secure Info">
	elastic password: <password>
	
	metricbeat_setup user: <See "Secure Info">
	metricbeat_monitoring user: <See "Secure Info">
	metricbeat_writer user: <See "Secure Info">
	metricbeat_reader user: <See "Secure Info">
	
	
	
 on a server in cluster:
	 /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s node
 	
	
	
password (temp) for elastic user on new clusters:
	See "Secure Info"	
	
	
sshfs elasticsearch@es01.lax.int.mailroute.net:/var/data/elasticsearch/tj /home/elasticsearch/mnt


Time to do a full backup (local) 25661s =  7 hours
time to do an incremental backup the next day = 207s = 3.5 minutes!
time to do an immediate incremental backup again = 41s :)

json for restore:
{
  "indices": "2023.06.17-*,2023.06.18-*,2023.06.19-*,2023.06.20-*,report_2023_*,report_virusnames",
  "feature_states": [
    "none"
  ]
}

time to do a full restore 2023.06.17 through 2023.06.20: 574s
	2023.06.17-*
	2023.06.18-*
	2023.06.19-*
	2023.06.20-*
	report_2023_*
	report_virusnames
	
Time to restore just last day + reports: 93s
(close these indexes before doing restore)
{
  "indices": "2023.06.20-amavisd,2023.06.20-policyd,2023.06.20-postfix,report_2023_amavisd,report_2023_policyd,report_2023_postfix,report_virusnames",
  "feature_states": [
    "none"
  ]
}
	2023.06.20-*
	report_2023_*
	report_virusnames

restore re-opens indexes automatically, btw



