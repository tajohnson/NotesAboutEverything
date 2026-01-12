

Amavisd logs are stored with a date with their expiration date.  If they need to be kept for 90 days, then they are stored with a log index name of "today + 90 days", etc.
Postfix and policyd logs are stored using todays date

So expiration routine is this:
	Delete amavisd logs older than today
	 Delete postfix logs older than <however many days we want to hold on to logs>


Keep postfix/policy logs for 180 days
