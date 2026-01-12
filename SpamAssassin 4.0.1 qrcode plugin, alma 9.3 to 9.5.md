

Update alma 9.3 to 9.5
	but breaks network
	So here's the routine
		rsync -avzr -e ssh root@199.89.0.103:/tmp/network /tmp/network/
		/tmp/network/save_network_info.sh
		dnf upgrade
		reboot
		hopefully at least one of the two interfaces comes up working - on test server it as the private that kept working
		/tmp/check_network.sh
		   which will call fix_network.sh if public is missing


Then update spamassassin to 4.0.1?

Then see if the plugin on 001.lax in /etc/mail/spamassassin/plugins/QRCode.pm is working
edit  /etc/mail/spamassassin/mroute.pre.  See if it works in spamassassin


Considerations - will also need to update elastic search?  Is this going to be an issue? Can we do a rolling server-by-server upgrade?



		