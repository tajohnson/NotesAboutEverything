Idea: a central location for monitoring mailservers, providing tools

Features:
	Blacklist checks
		One-off check

	Blacklist ongoing monitoring
		Provide a list of IP addresses and/or domain names and check blacklists for them.
		Free: 2 maximum, checked daily.
		Premium: different amounts, increase frequency of checking time, up to each minute?
		Monitor for changes
			Raise alert if changes
				Send email
				SMS (premium?)
				Pager Duty (premium)
		
			Display:
				IP/Domain Name:    Health Status,
				
	General SMTP server checks
		Reachable
		
	DNS Checks
		lookups of all types
		traceroute
		ping
		
	Mail Checks
		SPF
			check record
			wizard to build a record
			check to see if a message passes SPF?
				ie send email to address, get a report?
		DKIM
			check record given domain and selector
			check a message to see if it passes dkim?
				ie send email to an address, get a report
		DMARC
			check record
		
		


IDEA:  email health report
	Send email to 'healthreport@domain.com'
	Get back email that says
		SPF passed
		DKIM passed
		DMARC passed
		ADSP record exists
		Servers on blacklists
		SpamScore
		Path
		
		
		c
		
						
			
		
		

