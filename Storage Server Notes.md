Gluster
ZFS
	option 1. 12 disk pools - raidz2.  4tb disks = 40tb of usable space.
					can lose any 2 disks
					
	option 2.  6 disk pools - raidz1.  4tb disks = 40tb of space. 
					can lose 1 disk per pool
					faster IOPs
					
					
Either use servers with or without port expanders.
	Max xfer of a 4tb spinning disk is 227MB/s - 1.8Gbps
		24 drives max = 43.2Gbps
		12 drives max = 21.6 Gbps
	Max speed of an SAS bus is 6 Gbps
	
	Supermicro chassis with b don't have expanders.  One lane per drive.  Good for SSD
	Supermicro chassis with e16 expanders have just 1 SFF 8087 connector => max 24Gbps
	                        e26 expanders have just 2 SFF 8087 connectors => max 48GBps !!!
	                        



006.lax zfs on 4x4tb SATA drives


mirror 
zpool create mainpool mirror /dev/sdb /dev/sdc
zpool add mainpool mirror /dev/sdd /dev/sde
zpool list
zpool status

or

raidz array


zpool create mainpool raidz /dev/sdb /dev/sdc /dev/sdd /dev/sde

zfs create mainpool/mysql
zfs set mountpoint=/var/lib/mysql mainpool/mysql

zfs create mainpool/data
zfs set mountpoint=/data mainpool/data


optional:
# zfs set quota=500m mainpool/fs1
# zfs set reservation=200m mainpool/fs1


zfs create mainpool/logs
zfs set mountpoint=/var/logs mainpool/logs




zfs best practices 
- use /dev/disk/by-id/wwn* instead of other names for making zfs pools.




S01 Disk Mapping
	36 drive server
	Front:
		Bay  0 HGST HUS724040AL A580 PK1338P4HA72YB
		Bay  1 HGST HUS724040AL A580 PK1334PCKPGNWX
		Bay  2 HGST HUS724040AL A580 PK1334PCJNRT1X
		Bay  3 HGST HUS724040AL A580 PK1334PCK6UWKX
		Bay  4 HGST HUS724040AL A580 PK2338P4H9RALC
		Bay  5 HGST HUS724040AL A580 PK1338P4H85B9B
		Bay  6 HGST HUS724040AL A580 PK1338P4HA7R6B
		Bay  7 HGST HUS724040AL A580 PK1338P4HA00DB
		Bay  8 HGST HUS724040AL A580 PK1338P4HA74AB
		Bay  9 HGST HUS724040AL A580 PK2338P4HA52SC
		Bay 10 HGST HUS724040AL A580 PK2338P4HA546C
		Bay 11 HGST HUS724040AL A580 PK1338P4H9ZZLB
		
	Rear
		Bay 0 Samsung SSD 850 2B6Q S251NWAG307587J
		Bay 1 Samsung SSD 850 2B6Q S251NWAG307585M   
		Bay 2 Samsung SSD 850 2B6Q S250NSAG417883N
		Bay 3 Samsung SSD 850 2B6Q S250NSAG417884K
		
	
-----vdev_id.conf for s01.lax.mailroute.net
#     by-vdev
#     name     fully qualified or base name of device link
alias a0       /dev/disk/by-id/scsi-SATA_HGST_HUS724040A_PK1338P4HA72YB
alias a1       /dev/disk/by-id/scsi-SATA_HGST_HUS724040A_PK1334PCK9GNWX
alias a2       /dev/disk/by-id/scsi-SATA_HGST_HUS724040A_PK1334PCJNRT1X
alias a3       /dev/disk/by-id/scsi-SATA_HGST_HUS724040A_PK1334PCK6UWKX
alias a4       /dev/disk/by-id/scsi-SATA_HGST_HUS724040A_PK2338P4H9RALC
alias a5       /dev/disk/by-id/scsi-SATA_HGST_HUS724040A_PK1338P4H85B9B
alias b0       /dev/disk/by-id/scsi-SATA_HGST_HUS724040A_PK1338P4HA7R6B
alias b1       /dev/disk/by-id/scsi-SATA_HGST_HUS724040A_PK1338P4HA00DB
alias b2       /dev/disk/by-id/scsi-SATA_HGST_HUS724040A_PK1338P4HA74AB
alias b3       /dev/disk/by-id/scsi-SATA_HGST_HUS724040A_PK2338P4HA52SC
alias b4       /dev/disk/by-id/scsi-SATA_HGST_HUS724040A_PK2338P4HA546C
alias b5       /dev/disk/by-id/scsi-SATA_HGST_HUS724040A_PK1338P4H9ZZLB

# One zil per pool.  We have max of 5 pools or so on this server
# Parts 0-4 are for the ZIL for each pool. part 5 is for the l2arc
alias ssd0     /dev/disk/by-id/ata-Samsung_SSD_850_PRO_512GB_S250NSAG417883N
alias ssd1     /dev/disk/by-id/ata-Samsung_SSD_850_PRO_512GB_S250NSAG417884K



Then run this to build the links in /dev/disk/by-vdev

# vdevadm trigger


# SSD Caches:

For now, we're going to use the 256GB SSDs as a mirror for the boot/root file systems.

The 512GB SSDs are going to be used for both ZIL and l2arc.  Maybe we should add separate ZIL caches for each pool at some point - either a different disk for ZIL, or multiple disks for the 4 ZILs.

There are a couple different ssd caches for ZFS: 
	- l2arc read cache
		- Need about 1GB ram per 10GB of l2arc for index.
			- These first servers have 64GB ram, so they could handle maybe 500GB of l2arc, but we can easily add more ram, so let's use the most space available, and stripe the two 512GB drives for this.
			- 
	- ZIL write cache
		- Max required amount is .125 per GbE.  We have 2x10GbE, so we need 12.5GB of ZIL.
		- But we have 4 pools per server (4 6 disk pools max per server), and it's possible all writes could be hitting one, so we will set up 4 ZILs per server (let's make them 15GB each).
	
	We have a pair of 512GB drives for cache right now.  Use these for the first two pools.  Add another pair for the next two, I think, unless the l2arc turns out to not be of much benefit, in which case, maybe these can just do zil for all drives?
	
parted -s /dev/disk/by-id/ata-Samsung_SSD_850_PRO_512GB_S250NSAG417883N unit s mklabel gpt mkpart primary ext2 2048 15GB mkpart primary ext2 15GB 30GB mkpart primary ext2 30GB 271GB mkpart primary ext2 271GB 100%

parted -s /dev/disk/by-id/ata-Samsung_SSD_850_PRO_512GB_S250NSAG417884K unit s mklabel gpt mkpart primary ext2 2048 15GB mkpart primary ext2 15GB 30GB mkpart primary ext2 30GB 271GB mkpart primary ext2 271GB 100%






# create the pools
zpool create a raidz /dev/disk/by-vdev/a0 /dev/disk/by-vdev/a1 /dev/disk/by-vdev/a2 /dev/disk/by-vdev/a3 /dev/disk/by-vdev/a4 /dev/disk/by-vdev/a5

zpool create b raidz /dev/disk/by-vdev/b0 /dev/disk/by-vdev/b1 /dev/disk/by-vdev/b2 /dev/disk/by-vdev/b3 /dev/disk/by-vdev/b4 /dev/disk/by-vdev/b5

# add the ssd - zil
zpool add a log mirror ssd0-part1 ssd1-part1
zpool add b log mirror ssd0-part2 ssd1-part2

# l2arc (cache)
zpool add a cache ssd0-part3 ssd1-part3
zpool add b cache ssd0-part4 ssd1-part4


# for testing:

zfs create a/testa
zfs set mountpoint=/testa a/testa
zfs create b/testb
zfs set mountpoint=/testb b/testb

		

S02 Disk Mapping
	36 drive server
	Front:
		Bay  0 HGST HUS724040AL A580 PK1338P4HA9NTB
		Bay  1 HGST HUS724040AL A580 PK2338P4HA63RC
		Bay  2 HGST HUS724040AL A580 PK1338P4HA74EB
		Bay  3 HGST HUS724040AL A580 PK1338P4HA7TLB
		Bay  4 HGST HUS724040AL A580 PK1338P4HA8PGB
		Bay  5 HGST HUS724040AL A580 PK2338P4H9940C
		Bay  6 HGST HUS724040AL A580 PK2338P4HA63PC
		Bay  7 HGST HUS724040AL A580 PK1338P4HA8J6B
		Bay  8 HGST HUS724040AL A580 PK2338P4HA14WC
		Bay  9 HGST HUS724040AL A580 PK1338P4HA73SB
		Bay 10 HGST HUS724040AL A580 PK1334PCK9ZXBX
		Bay 11 HGST HUS724040AL A580 PK1338P4HA032B
		
	Rear
		Bay 0 Samsung SSD 850 2B6Q S251NWAG307583W
		Bay 1 Samsung SSD 850 2B6Q S251NWAG307584P  
		Bay 2 Samsung SSD g PRO 512GB 2B6Q S419NGAK110560Y /dev/sdo
		Bay 3 Samsung SSD 850 2B6Q S419NGAK111981B  /dev/sdp
		
-----vdev_id.conf for s01.lax.mailroute.net
#     by-vdev
#     name     fully qualified or base name of device link
alias a0       /dev/disk/by-id/scsi-SATA_HGST_HUS724040A_PK1338P4HA9NTB
alias a1       /dev/disk/by-id/scsi-SATA_HGST_HUS724040A_PK2338P4HA63RC
alias a2       /dev/disk/by-id/scsi-SATA_HGST_HUS724040A_PK1338P4HA74EB
alias a3       /dev/disk/by-id/scsi-SATA_HGST_HUS724040A_PK1338P4HA7TLB
alias a4       /dev/disk/by-id/scsi-SATA_HGST_HUS724040A_PK1338P4HA8PGB
alias a5       /dev/disk/by-id/scsi-SATA_HGST_HUS724040A_PK2338P4H9940C
alias b0       /dev/disk/by-id/scsi-SATA_HGST_HUS724040A_PK2338P4HA63PC
alias b1       /dev/disk/by-id/scsi-SATA_HGST_HUS724040A_PK1338P4HA8J6B
alias b2       /dev/disk/by-id/scsi-SATA_HGST_HUS724040A_PK2338P4HA14WC
alias b3       /dev/disk/by-id/scsi-SATA_HGST_HUS724040A_PK1338P4HA73SB
alias b4       /dev/disk/by-id/scsi-SATA_HGST_HUS724040A_PK1334PCK9ZXBX
alias b5       /dev/disk/by-id/scsi-SATA_HGST_HUS724040A_PK1338P4HA032B

# One zil per pool.  We have max of 5 pools or so on this server
# Parts 0-4 are for the ZIL for each pool. part 5 is for the l2arc
alias ssd0     /dev/disk/by-id/ata-Samsung_SSD_860_PRO_512GB_S419NGAK110560Y
alias ssd1     /dev/disk/by-id/ata-Samsung_SSD_860_PRO_512GB_S419NGAK111981B



Then run this to build the links in /dev/disk/by-vdev

# udevadm trigger


parted -s /dev/disk/by-id/ata-Samsung_SSD_860_PRO_512GB_S419NGAK110560Y unit s mklabel gpt mkpart primary ext2 2048 15GB mkpart primary ext2 15GB 30GB mkpart primary ext2 30GB 271GB mkpart primary ext2 271GB 100%

parted -s /dev/disk/by-id/ata-Samsung_SSD_860_PRO_512GB_S419NGAK111981B unit s mklabel gpt mkpart primary ext2 2048 15GB mkpart primary ext2 15GB 30GB mkpart primary ext2 30GB 271GB mkpart primary ext2 271GB 100%






# create the pools
zpool create a raidz /dev/disk/by-vdev/a0 /dev/disk/by-vdev/a1 /dev/disk/by-vdev/a2 /dev/disk/by-vdev/a3 /dev/disk/by-vdev/a4 /dev/disk/by-vdev/a5

zpool create b raidz /dev/disk/by-vdev/b0 /dev/disk/by-vdev/b1 /dev/disk/by-vdev/b2 /dev/disk/by-vdev/b3 /dev/disk/by-vdev/b4 /dev/disk/by-vdev/b5

# add the ssd - zil
zpool add a log mirror ssd0-part1 ssd1-part1
zpool add b log mirror ssd0-part2 ssd1-part2

# l2arc (cache)
zpool add a cache ssd0-part3 ssd1-part3
zpool add b cache ssd0-part4 ssd1-part4

zfs set mountpoint=none a
zfs set mountpoint=none b

# for gluster:

zfs create a/brick1
zfs create b/brick2

zfs set mountpoint=/data/glusterfs/mail/brick1 a/brick1
zfs set mountpoint=/data/glusterfs/mail/brick2 b/brick2


		

		
		
		
# current dovecot (no gluster)

 zfs create a/mailhome
 zfs create a/mailindices
 zfs create b/mailstorage
 
 zfs set mountpoint=/data/mailhome a/mailhome
 zfs set mountpoint=/data/mailindices a/mailindices
 zfs set mountpoint=/data/mailstorage b/mailstorage

zfs create a/postfix-deliver
zfs set mountpoint=/data/postfix-deliver a/postfix-deliver



 # s02.lax.mailroute.net - elasticsearch
 zfs create a/elasticsearch_data
 zfs create b/elasticsearch_data
 
 zfs set mountpoint=/data/elasticsearch_data/0 a/elasticsearch_data
 zfs set mountpoint=/data/elasticsearch_data/1 b/elasticsearch_data
 
 zfs create a/elasticsearch_logs
 zfs create b/elasticsearch_logs

 zfs set mountpoint=/data/elasticsearch_logs/0 a/elasticsearch_logs
 zfs set mountpoint=/data/elasticsearch_logs/1 b/elasticsearch_logs

 
 
 
 

