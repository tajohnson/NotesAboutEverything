

could info bar include a unique message-id for each message that could be used to find a copy of the message kept for a short time to be able to diagnose problems?  Maybe a small one at the end of the message that isn't noticeable?





Idea:
	
	Optionally add an informational line to every email that's coming in from the outside.  This can help people spot external emails, warn about phishing, make info available to the user that's otherwise hidden in the headers (and not displayed by most mail clients)..
	
	From Header:
	Envelope From:
	(note mismatch?)
	Sent from: (first external gateway?)
	Spam Score:
	Spam Status:
	X-Virus Scanned:
	Spam rules?
	DKIM
	DMARC
	SPF
	TLS Used in transfer to us?
	Attachment Types
	Email Size
	Bulk Mail?
	Mailing List
	Transactional Email?
	Languages
	Countries
	Unsubscribe Link:
	Blacklist sender?
	Whitelist Sender?
	Report as spam?
	
	
	
Gotchas:
	some emails may be base64 encoded, and it decodes to html (paypal)
		
	