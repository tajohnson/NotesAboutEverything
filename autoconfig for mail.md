# autoconfig for mail

autoconfig.domain.com
	haproxy passes http traffic on port 80 through without https redirect
	then on control panel mailservers:
		/var/www/autoconfig
			mail/config-v1.1.xml - for thunderbird
			
		Thunderbird loads this to set server config automatically
	customers can set up
		autoconfig CNAME autoconfig.mailroute.net.
		
	Then their users can use 