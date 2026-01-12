### IMAP xfer feature

https://github.com/imapsync/imapsync
/usr/bin/imapsync --host1 imap.islandemail.com --user1 tj@terramar.net --password1 MASKED --host2 imap.mailroute.net --user2 tj@terramar.net%master --password2 MASKED --maxsize  5242880000

		--automap      : guesses folders mapping, for folders well known as
                    "Sent", "Junk", "Drafts", "All", "Archive","Flagged".
		--nossl1    disabled ssl on source host
		--notls1    disable tls on source host
		--skipemptyfolders
		--include      reg  : Sync folders matching this regular expression
	     --include      reg  : or this one, etc.
                           If both --include --exclude options are used, then
                           include is done before.
	     --exclude      reg  : Skips folders matching this regular expression
                           Several folders to avoid:
                            --exclude 'fold1|fold2|f3' skips fold1, fold2 and 
		--tmpdir
		--pidfile
	     --disarmreadreceipts : Disarms read receipts (host2 Exchange issue)
	          --maxsize      int  : Skip messages larger  (or equal) than  int  bytes
	     --minsize      int  : Skip messages smaller (or equal) than  int  bytes

	     --maxage       int  : Skip messages older than  int days.
                           final stats (skipped) don't count older messages
                           see also --minage
	     --minage       int  : Skip messages newer than  int  days.
		   --gmail1           : sets --host1 to Gmail and other options. See FAQ.Gmail.txt
		 --office1          : sets --host1 to Office365 and other options. See FAQ.Office365.txt
		   --exchange1        : sets options for Exchange. See FAQ.Exchange.txt
		 --domino1          : sets options for Domino. See FAQ.Domino.txt
		   --emailreport2     : Put the email final report in host2 INBOX


- icloud:
	Use "imap.mail.me.com" as the imap server and use an app password.
	How to generate an app password is described at
	https://support.apple.com/kb/HT204397
 - yahoo
	Use --host1 imap.mail.yahoo.com 
		You also need to go to Yahoo, security and enable 
		"Allow less secure apps to login", 
		otherwise the login will not work. 
		To enable less secure apps to login:
			  * Login to the Yahoo mail account,
			  * click on the account name or the avatar and select "Account Info",
			  * click on "Account security",
			  * turn off "Two steps verification"
			  * turn on "Allow apps that use less secure authentication".​
		  * Another way to do this
			  * Login to the Yahoo mail account,
			  * click on the account name or the avatar and select "Account Info",
			  * click on "Account security",
			  * Turn on "Two-step verification"
			  * Click on "Manage app passwords" and 
			    generate a specific password for imapsync, 
				choose "Other app" at the bottom and type imapsync
			    since it is not in the predefined apps.
			  * Use this password with imapsync.
- Exchange
	- R0. IMAP is not enable by default on Exchange, see how to enable it:
		https://docs.microsoft.com/en-us/exchange/enable-imap4-in-exchange-2013-exchange-2013-help
		Also read
		http://clintboessen.blogspot.com/2018/03/binding-certificate-breaks-imap-or-pop.html
		
		R1. Disable double-step authentication, also known as 2-factor,
	    2-step authentication on the Azure/Active Directory portal.



- zimbra
	- can use an "admin account" to access users without their specific passwords
	-   imapsync ... --user1 "normal_user" --authuser1 "admin_user"  --password1 "admin_user_password"
	- To setup or use a Zimbra admin user see:
https://zimbra.github.io/adminguide/8.8.9/index.html#_administrator_accounts


