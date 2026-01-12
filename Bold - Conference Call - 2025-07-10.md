
#### Questions for Varun:

1. Exactly what types of users are using this? 
	1. Bold employees?
	2. Enterprises?
	3. End-users?
2. How are they using it? Receiving? Sending internally? Externally? Both?
3. Will they be on a different domain or a subdomain?
	1. ex: ACM uses @acm.org for end users, hq.acm.org for employees
4. Quota requirements
	1. Typical is 50GB per user, $0.25 per gb per month for overage
	2. Typical sending allowance 
	3. How do you see using our API for user management, settings, etc.
	4. Do you need API access to email in people's email boxes - please explain the use
5. Custom filtering
	1. Additional Categories?
	2. What types of filtering are required beyond our standard spam/malware/etc types?


### Requirement
provide end users way to connect to recruiters
	apply to job, emails between recruiter <--> job seeker
	 They want to be able to intercept that info
	 Separate email box for end user, so it's all kept separate
	 different aliases, only used for job-related interactions
	 view all interactions - 
	 api support to read those emails so they can automate job application process from your side.
	 - restrict email access to job seekers, recruiters, and you
- Quota: perhaps hundreds to thousands of emails, mostly textual, some resumes, PDFs, 


### share architecture of platform so Varun can understand what we do in more detail to figure out how to integrate it.
### provide documentation for adding domains, testing us

### timeframe: looking at vendors right now, want to start ASAP.


#### Thoughts:
* still need to do filtering - users and recruiters could spam or send malware
## Further questions
 - You mentioned users having "aliases" - is this multiple aliases per user? Or one per user?
 - Is ALL traffic on this system local? Is it only allowing email between your recruiters and job seekers?  Or is there a path into this system from outside?  For example, are users forwarding mail to this system from the outside?
 - Job seekers and recruiters - interface - Do you want them to use a regular email client for this?  Do you want them to have a webmail interface for it?  We can provide both of those.  
   Or do you need a customized "webmail-like" interface for them to use?  We could create that to your specifications.

Dashboard



