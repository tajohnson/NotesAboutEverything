# Steps right before the move:


- [ ] Move logstash....
- [ ] move continuity to es04.lax
	- [ ] copy data from s01.lax
	- [ ] stop dovecot on s01
	- [ ] rsync incremental
	- [ ] start dovcecot on es04
	- [ ] change loadbalancer
	- [ ] change continuity.mailroute.net roundcube if needed
- [ ] Get 005.mia and 006.mia working as filters to be backup
- [ ] What else might be running on s01 or s02 besides logstash and dovecot?
	- [ ] is anything using redis off s02.lax?  If so, change it
	- [ ] 











Melissa - 314-485-7114
Catherine - 720-826-6866 






Questions:
- When is the deadline

	

-What goes down when LA goes down
	
		
	- memcached - sort of fixed with a backup 
	- elasticsearch - can't fix that right now
	- continuity - probably can't fix that right now
	- hosted mail - get foundationdb working properly distributed and put some stalwart servers in MIA (maybe lease a couple more servers elsewhere as coordinators/satellite logs)


### Plan: 
- [x] Get a server in Dallas to be redis sentinel, satellite log/coordinator for foundationdb
- [ ] Another one? Maybe in Austin to be fully safe?
- [ ] Make es01, es02, 001.mia, 002.mia foundationdb for hostedmail
	- [ ] How many foundationdb processes? Thinking 4 per server to start
- [ ] Make es03, es04, 003.mia, 004.mia foundationdb for continuity
- [ ] Order servers for miami
	- [ ] 
- [ ] 


### Redis

	Can we go to cluster? Or must we use sentinel?  If sentinel, then master in mia, slave in LA, and another in a rented space somewhere like where we have remote.mroute.net
	This could work - haproxy to send stuff to master, but then use sentinel to handle promotion
			sentinel handles promtion of replica to master during  a failure
			haproxy checks for "master" role and marks others as "down".
		```


#### Set up Redis Sentinel
- [x] redis is now on 002.mia.int.mailroute.net and 005.lax.mailroute.net
- [x] haproxy set up for redis
- [ ] set up sentined - 002.mia, 005.lax, remote.mroute.net?
- [ ] redirect everything to use this redis
- [ ] 

ElasticSearch
	Can we move to OpenSearch? Move to Miami, then set up actual cross datacenter replication between the two?
	Possible plan:
	  - decommision 2 es servers
	  - redo them as opensearch servers
	  - migrate from es8 to opensearch (cannot do a simple backup/restore becuase of version mismatch)
	  - purchase and install 2 new es servers in MIA - set up opensearch on them
	  - set up cross-datacenter replication for the opensearch servers
	  - 
	  - Purchase 2 new servers for MIA?

#### Hosted Mail
- [x] Move foundationdb to multi-datacenter
- [x] Install stalwart on on server(s) in MIA
- [ ] Add to haproxy
- [ ] test
- [ ] 

#### Continuity
	Can we move from dovecot to stalwart in miami?
	Can we do a server in LA and one in MIA and use foundationdb to replicate data between datacenters?


Do we need more servers?
- [ ] perhaps 2 filtering servers for MIA for capacity?
- [ ] perhaps 2 es01-class servers for hostedmail, continuity in MIA
	- [ ] 



### foundation db for hosted and continuity
##### hosted mail
	es01.lax
	es02.lax
	001.mia
	002.mia
##### continuity
	es03.lax
	es04.lax
	003.mia
	004.mia



Dallas option
	1. Colocation - $500/month + $10k for servers?
		1. Put a couple ES servers there (after moving from ES to OS)
		2. Put a couple servers there for foundationdb
	2. Bare metal
		1. Get two servers from a Dallas provider and maybe an Austin provider for foundation
			1. $200/month
		2. ElasticSearch ?





# What has to be changed to stop using private network and only use public

firewall - do I disable it for now?  I think it's okay - accepting stuff from 199.89.0.0/21 in trusted zone



- [ ] /etc/resolv.conf
- [ ] /etc/mail/spamassassin/local.cf
- [ ] /etc/sysconfig/rbldnsd
- [ ] /etc/sysconfig/memcached
- [ ] /etc/ssh/sshd_config.d/99-mroute.conf
- [ ] /etc/ssh/ssh_known_hosts
- [ ] /etc/amavisd/amavisd.conf
- [ ] /etc/postfix-inbound/main.cf
- [ ] /etc/postfix-outbound/main.cf
- [ ] /etc/postfix-relayout/main.cf
- [ ] /etc/postfix-relayisp/main.cf
- [ ] /etc/postfix-relaycontinuity/main.cf
- [ ] /etc/postfix-forward/main.cf
- [ ] /etc/postfix-archive/main.cf
- [ ] /etc/authentication_milter.json
- [ ] /etc/postfix-smtpauth/main.cf
- [ ] /etc/redis/redis.conf

# redis
- [x] server
- [x] clients
	- [ ]amavisd
	- control panel
	- spamassassin
# playbooks

all.yml
controlpanel.yml
datmaster.yml
filters.yml
imap.yml
kibana.yml
loadbalancer.yml
named.yml
search.yml
setup.yml
vault.yml
webmail.yml


filebeat


### done
redis.yml
memcached.yml


Run these playbooks on all servers:
	filters.yml