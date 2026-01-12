Redis

TODO - move all to new redis set up on 012.lax.int.mailroute.net (which replicates to 002.mia, by the way),  Be sure to update logstash as well!

(note - had to do new redis for decodeshorturls - need to update everything else to use the new redis servers!)

Databases:
				0 - default (not used) - if anything ends up in here, it's misconfigured
				1 - celery broker (production only - don't use with other instances
				2 - production amavisd-new (penpals, logging (ingested by logstash))

				3 - webapp - production
				4 - webapp - dev
				5 - webapp - qa
				6 - webapp - ui2

				7  - unused
				8 - mroute decode urls url shortener
				9 - spamassassin (for bayes, etc)
				10 - unused
				11 - unused
				12 - unused
				13 - unused
				14 - unused
				15 - unused
				
				
				
				
TODO - implement HA for redis


Requires multi-master or redis/dynomite across datacenters:
	
	amavisd penpals
	mailroute admin
	celery/admin broker
	spamassassin
	
	RAM requirement - estimated 2-4GB

Does not require multi-master - can be separate instances in each DC
	amavisd-new logging
	
	RAM requirement - unknown
		
	
	