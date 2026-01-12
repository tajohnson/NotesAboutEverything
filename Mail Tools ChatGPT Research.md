
2025-06-05



##### You said:

I want to create a small service to offer a set of email-related tools to people. Some will be free. Some will be paid. Some may be accessible directly, some might require an account and login. These are tools to help email admins. I am thinking of things like free: mx record lookup. Spf record lookup. Test connection to email server. Test to see if email server is open relay. Paid: spf record flattening. Mailserver connectivity (ie, send an email, and give an alert if email not recieved in a given time frame). To do this, I need your help. Can you research and find at least 3-7 different services or webpages offering these features, and give me a complete list of tools that can be offered? We're going to create our own list of tools and want to know what we need to offer so that our service is as complete as, or more complete than the competition. Then make a recommendation on which tools should be free and which should be paid. Free tools might be useful to nudge people to use the paid service. Make a recommendation on pricing for the service. Are multiple tiers needed? if so, how should they be set up - what do you get for higher paid tiers? Suggest the order of implementation of these tools. Perhaps starting with a number of free tools, and a simple paid service (like spf record flattening), and then adding more tools until the service is complete. This information is going to be used to provide chatgpt with the details to help build both the front and backends of this service. So provide the info in a format suitable to providing back to an llm, such as in markdown format Before starting, be sure to ask as many clarifying questions as you can think of.

###### ChatGPT said:

Thanks for the detailed overview! To make sure the research is as useful as possible, could you please clarify the following:

1. **Target Audience**: Are you focusing on small businesses, large enterprises, freelance email admins, MSPs, or a mix?
    
2. **Technology Stack Preference**: Do you already have a preferred backend (e.g., Python, Node.js, Go) or hosting platform (e.g., AWS, Vercel, self-hosted)?
    
3. **Geographic Considerations**: Is your audience primarily in a specific region (e.g., US/EU) that may affect compliance features (like GDPR)?
    
4. **Tool Access Methods**: Should these tools be accessible via API as well as a web UI?
    
5. **Branding/UX**: Do you envision the service as white-labeled for teams, or more like a public utility/toolbox site?
    
6. **Competitor Preference**: Any known competitors you'd like us to prioritize analyzing (e.g., MXToolbox, DNSstuff, DNSChecker)?
    

Once I have these, I can start the research and compile a full competitive analysis and feature roadmap in markdown format.

##### You said:

1. A mixture of small and large businesses and MSPs. I would assume that the free tools will likely be used by freelance admins too. Maybe things like the mailserver connectivity monitoring would be used by freelance admins too. 2. Open to ideas for backend and hosting. 3. US/EU to start. GDPR is a consideration. 4. API and web UI 5. More like a public untility/toolbox site. 6. mxtoolbox.com is the biggest one I can think of. dmarcian.com is another. I need you to find others. Don't use dnsstuff. https://dnschecker.org/all-tools.php But I need you to find me a bunch more. Find at least 5-7 companies besides mxtoolbox and dmarcian

###### ChatGPT said:

Great. I’ll research at least 7 services that offer email-related tools similar to MXToolbox and Dmarcian. I’ll compile a list of all tools they offer—both free and paid—and use that to recommend a complete feature set for your service. I’ll also suggest which tools should be free vs paid, pricing tiers, and an implementation roadmap to help guide your backend and frontend development.

Research completed in 9m · 25 sources · 99 searches

Competitive Analysis of Email Admin Tools and Feature Roadmap

# Email Administration Tools Research and Recommendations

## Competitor Services and Tool Offerings

Below we analyze several widely-used email administration tool providers (excluding MXToolbox and dmarcian) that offer a mix of free and paid tools for email administrators. For each service, we list the tools they provide and note which are free versus part of paid offerings.

### DNSChecker (DNSChecker.org)

- **Tools Offered:** Comprehensive DNS and email record lookup tools. This includes **MX record lookup**, **SPF record checker**, **DKIM record lookup**, **DMARC record lookup/validation**, **DMARC record generator**, **BIMI record lookup**, **DNS propagation checker**, **DNS Health Checker**, **WHOIS lookup**, **reverse IP lookup**, **IP blacklist checker**, **email header analyzer**, **port checker**, and more[dnschecker.org](https://dnschecker.org/dkim-record-checker.php#:~:text=Related%20tools%20DMARC%20Generator%20,Record%20Check%20%20%2011)[dnschecker.org](https://dnschecker.org/all-tools.php#:~:text=SPF%20Record%20Checker%20Lookup%20and,Validate%20SPF%20Record%20of%20Domain). Essentially, DNSChecker provides a full DNS toolbox covering all DNS record types and many network tools.
    
- **Free vs. Paid:** All tools on DNSChecker are **free to use** with unlimited queries. DNSChecker explicitly states that lookups (e.g. the MX lookup) are entirely free with no hidden limits[dnschecker.org](https://dnschecker.org/mx-lookup.php#:~:text=MX%20Lookup%20,check%20their%20mail%20exchange%20records). The site appears ad-supported; there’s no mention of paid subscriptions or API access on their site (no premium tier is advertised). Thus, DNSChecker serves as a free utility collection for DNS/email lookups.
    

### IntoDNS (IntoDNS.com)

- **Tools Offered:** IntoDNS provides an **all-in-one domain health report** focusing on DNS and mail server configuration. When you enter a domain, it runs a series of checks on DNS records (NS records, SOA settings, etc.) and **mail server checks** for that domain[intodns.com](https://intodns.com/#:~:text=intoDNS%3A%20checks%20DNS%20and%20mail,fix%20and%20improve%20them%2C). It reports on MX records (presence, validity, duplicate records, PTR records), DNS server settings (glue records, recursion, consistency), **SPF record checks** (correct format, only one SPF record, etc.), **DMARC record checks** (presence and correct syntax), and other DNS best practices[intodns.io](https://intodns.io/api-docs/v1/#:~:text=,55)[intodns.io](https://intodns.io/api-docs/v1/#:~:text=IntoDNS%20V1%20API%20docs). In essence, IntoDNS combines multiple tests into a single detailed **DNS and mail server health report**with recommendations.
    
- **Free vs. Paid:** All features of IntoDNS are **completely free**. The service does not require login and has no paid tier. In fact, an official IntoDNS API is provided for free as well (with usage limits of 100 requests/minute)[intodns.io](https://intodns.io/api-docs/v1/#:~:text=Usage%20limits%20and%20terms%20of,use%20and%20disclaimer)[intodns.io](https://intodns.io/api-docs/v1/#:~:text=Welcome%20to%20the%20IntoDNS%20API%21,This%20is%20a%20free%20API). There are no premium plans; IntoDNS seems to be supported by the creators (a hosting company) as a free community tool.
    

### Google Admin Toolbox (toolbox.googleapps.com)

- **Tools Offered:** Google’s Admin Toolbox is a set of technical troubleshooting tools, several of which are useful for email admins. Key tools include **“Check MX”**, which verifies a domain’s MX records and related email DNS records. The Check MX tool not only lists MX records but also checks for **SPF**record presence, **DKIM** keys, **DMARC** policy, and even flags issues like too many DNS lookups in SPF or missing DNSSEC/MTA-STS records[dmarcreport.com](https://dmarcreport.com/blog/learning-to-setup-spf-dkim-and-dmarc-in-google-workspace/#:~:text=Learning%20to%20Setup%20SPF%2C%20DKIM%2C,Although)[support.duda.co](https://support.duda.co/hc/en-us/articles/26649783401367-Google-s-Email-Sender-Guidelines#:~:text=Google%27s%20Email%20Sender%20Guidelines%20,Up%20DKIM%20TXT%20Records). Other tools in the suite: **Dig** (for DNS queries), **Messageheader** analyzer (to parse and analyze email message headers for delays or authentication results), **Useragent** and **Browserinfo** (for client debugging), **HAR Analyzer** and **Log Analyzers** (for network log troubleshooting)[toolbox.googleapps.com](https://toolbox.googleapps.com/apps/checkmx/#:~:text=Google%20Admin%20Toolbox%20%20home,Home). These tools are particularly useful for diagnosing email delivery issues (e.g., Google’s Check MX will warn if DKIM or DMARC is not set up properly[support.google.com](https://support.google.com/a/thread/302411315/google-admin-toolbox-check-mx-shows-dmarc-dkim-is-not-setup-but-it-is?hl=en#:~:text=check%20MX%20shows%20DMARC%2FDKIM%20is,been%20for%20a%20long%20time)).
    
- **Free vs. Paid:** All Google Admin Toolbox tools are **completely free**. There is no paid version – Google provides these tools as a free resource. There are also no usage limits published; anyone can use Check MX, Dig, etc. for unlimited domains. (Google’s motive is to help administrators troubleshoot, especially for Google Workspace setups, without a direct monetization of the toolbox.)
    

### CheckTLS (CheckTLS.com)

- **Tools Offered:** CheckTLS specializes in **mail server connectivity and security testing**, especially focusing on SMTP TLS encryption and server configuration. Its primary tools are: **“Test Receiver”**which tests the TLS/SSL capabilities of an email server by simulating an SMTP connection to a given email address or mail server (checking for encryption, correct certificates, etc.), **“Test Sender”**which evaluates how your outbound email (SMTP client) is performing, and various other email security tests (for example, it can test if a server is an open relay, verify a server’s certificate chain, and check email authentication on a low level). Essentially, CheckTLS allows an admin to perform an end-to-end test of sending an email to a recipient and see where it might fail or if it’s properly secured. They also offer continuous email system monitoring (to regularly test that your email servers can send/receive securely).
    
- **Free vs. Paid:** CheckTLS offers a basic level of testing for free on their website (you can run ad-hoc tests via their forms with default settings). Advanced features, bulk/automated testing, and monitoring require a **subscription**. They have personal and corporate subscription levels. For instance, a Professional subscription is about **$10 per month or $100/year** for full access to all test types and features[checktls.com](https://www.checktls.com/aboutsubscription.html#:~:text=,Payment). Higher tiers like Corporate (around $25/month or $250/year) allow multiple users, automated monitoring, API access and other enterprise features[checktls.com](https://www.checktls.com/SubscriptionSignUp#:~:text=%2F%2Fsubscription%2Fsignup%20,USD%29). In summary, one-off tests are free (with some limitations on options), whereas automation, saved tests and integration require a paid plan.
    

### HetrixTools (HetrixTools.com)

- **Tools Offered:** HetrixTools is a service focused on **blacklist monitoring and uptime monitoring**, with specific appeal to email admins for its blacklist check features. Key tools and features include: an **Email/IP Blacklist Checker** that can instantly check an IP address or domain against _over 100 DNS-based blacklists_ (RBLs) on demand[hetrixtools.com](https://hetrixtools.com/mxtoolbox-alternative/#:~:text=checked%20every%207%20days%2C%20on,just%2030%20RBLs), **Bulk IP blacklist checks** (you can input or upload multiple IPs to check at once)[hetrixtools.com](https://hetrixtools.com/mxtoolbox-alternative/#:~:text=checked%20every%207%20days%2C%20on,just%2030%20RBLs), and continuous **Blacklist Monitoring** where the system will monitor a set of IP addresses or domains on a schedule and alert you if any get listed (the free tier monitors once per day). They also provide **uptime monitoring** (HTTP/SSL pings, etc.) and even integration with Microsoft SNDS for IP reputation, but the core email-admin focus is on blacklist status. HetrixTools also offers features like public **status pages** (for service uptime) and **server monitoring** agents, though those are more tangential to email administration.
    
- **Free vs. Paid:** HetrixTools uses a freemium model. The **free tier** is quite generous for blacklist monitoring: _free forever_ for up to 32 blacklist monitors (e.g., 32 IPs or domains) checked every 24 hours, plus up to 15 uptime monitors, etc.[hetrixtools.com](https://hetrixtools.com/#:~:text=15%20Uptime%20Monitors%20%E2%80%A2%201,keep%20it%20clean%20from%20spam). All on-demand tools (manual blacklist lookup, etc.) are free with an account. Paid plans mainly increase the quotas and check frequency. For example, the **Professional plan** is $9.95/month and offers 256 blacklist monitors (and faster checking interval), the **Business plan** $19.95/month for more, and custom Enterprise plans for very large needs[hetrixtools.com](https://hetrixtools.com/mxtoolbox-alternative/#:~:text=,0388%2FIP%2Fmonth)[hetrixtools.com](https://hetrixtools.com/mxtoolbox-alternative/#:~:text=,days%2C%20on%20just%2030%20RBLs). HetrixTools emphasizes that their pricing per monitored IP is far cheaper than some competitors[hetrixtools.com](https://hetrixtools.com/mxtoolbox-alternative/#:~:text=,0388%2FIP%2Fmonth). In summary, basic blacklist and uptime monitoring is free (with limits), and higher capacity or more frequent checks are paid.
    

### GlockApps (GlockApps.com)

- **Tools Offered:** GlockApps is an email deliverability and testing platform. It provides a suite of tools oriented towards ensuring emails reach the inbox. Notable tools include: **Inbox placement testing** – you send an email to a special seed list of addresses and GlockApps reports how many went to Inbox vs Spam at various providers (paid feature), **Spam score testing** (they check your email against spam filters and give a score), and **DMARC analytics/monitoring**. GlockApps also makes available free lookup tools on their website to draw users: an **MX record checker**[glockapps.com](https://glockapps.com/mx-record-checker/#:~:text=Free%20MX%20Checker%3A%20Lookup%20Your,into%20your%20mail%20server%20setup), **SPF record checker**[glockapps.com](https://glockapps.com/spf-record-checker/#:~:text=SPF%20Checker%20Tool%20,record%20lookup%20and%20validation), **DMARC record checker**[glockapps.com](https://glockapps.com/dmarc-record-checker/#:~:text=DMARC%20Checker%20Tool%20,email%20spoofing%20and%20phishing%20attacks), and a general **Domain SMTP configuration “health” checker** that validates SPF, DKIM, DMARC and other sending-related configs[glockapps.com](https://glockapps.com/domain-checker/#:~:text=Use%20free%20GlockApps%27%20Domain%20Checker,records%20for%20optimal%20email). Additionally, their DMARC analytics service includes tools to generate records and to process DMARC aggregate reports. GlockApps essentially combines technical email authentication tools with campaign performance tools (for marketers).
    
- **Free vs. Paid:** GlockApps has **free tools** (the record lookup/checker tools mentioned above are free on the website without login). They also offer a limited free tier for DMARC monitoring – for example, they allow processing of up to **10,000 DMARC aggregate reports free each month** for DMARC monitoring[glockapps.com](https://glockapps.com/dmarc-analyzer/#:~:text=DMARC%20Checker%20,with%20GlockApps%20DMARC%20Analyzer%20Tool). However, the core deliverability testing features and higher-volume monitoring require **paid plans**. Their pricing is tiered: for instance, the **Basic plan is around $79/month** (if paid monthly) which includes ~300 spam test credits and monitoring of 100k DMARC messages, among other features[g2.com](https://www.g2.com/products/glockapps/pricing#:~:text=Basic,Monitors%3B%201%20User%3B%20All). Higher plans (Growth, Enterprise) cost more (e.g. $99, $129/month...) and increase the number of tests, domains, and DMARC report volume included[g2.com](https://www.g2.com/products/glockapps/pricing#:~:text=Basic,Monitors%3B%201%20User%3B%20All)[subscribed.fyi](https://subscribed.fyi/glockapps/pricing/#:~:text=GlockApps%20Pricing%20and%20Features%20overview,Enterprise%20Plan%3A%20%24129%2Fmonth%2C). They also include extra features like IP reputation monitors and uptime monitors at those tiers[g2.com](https://www.g2.com/products/glockapps/pricing#:~:text=Basic,Monitors%3B%201%20User%3B%20All). In short, GlockApps lures users with free record checks and a free DMARC quota, but the full deliverability suite is paid with multi-tier subscriptions.
    

### EasyDMARC (EasyDMARC.com)

- **Tools Offered:** EasyDMARC is a DMARC-focused email security platform that also provides a broad set of email admin tools. On the **free tools** side, EasyDMARC offers an entire toolkit: **Domain Scanner & Health Check** (a free one-click audit of a domain’s SPF, DKIM, DMARC, MX, and general email security posture)[easydmarc.com](https://easydmarc.com/tools/domain-scanner#:~:text=EasyDMARC%20Domain%20Scanner%20and%20Health,your%20domain%20in%20one%20click)[easydmarc.com](https://easydmarc.com/tools#:~:text=Free%20tools%3B%20Domain%20Scanner%20%C2%B7,Record%20Checker%20%C2%B7%20All%20tools), **DMARC record lookup** and **DMARC record generator**, **SPF record lookup** and **SPF generator**[easydmarc.com](https://easydmarc.com/tools#:~:text=Free%20tools%3B%20Domain%20Scanner%20%C2%B7,Record%20Checker%20%C2%B7%20All%20tools), **DKIM record checker** and a DKIM setup guide, **BIMI checker/generator** for brand indicators, a **DNS record lookup tool**, and even a **phishing link checker** and basic **IP/domain reputation checker**[easydmarc.com](https://easydmarc.com/tools#:~:text=Free%20tools%3B%20Domain%20Scanner%20%C2%B7,Record%20Checker%20%C2%B7%20All%20tools). Essentially, EasyDMARC’s website provides nearly all the record generation and validation tools an email administrator would need for authentication and security.
    
- **Free vs. Paid:** All the above-mentioned tools are available free on their site (no login required for most). EasyDMARC’s revenue model comes from its **DMARC reporting and monitoring service**. They offer a free tier for DMARC reporting as well: processing up to **10,000 emails/month in DMARC aggregate reports for free**[easydmarc.com](https://easydmarc.com/pricing/easysender/inbox-placement#:~:text=DMARC%20Report%20Reader%2C%20Analyzer%20,the%20best%20DMARC%20tools%2C). Beyond that, paid plans kick in – **Plus plan at ~$35.99/month** (covers higher email volume, a couple of domains, and longer data retention) and **Premium at ~$71.99/month** for more volume and domains[g2.com](https://www.g2.com/products/easydmarc/pricing#:~:text=EasyDMARC%20Pricing%202025%20EasyDMARC%20Pricing,Data%20History%3B%201%20User). (These prices are for businesses; MSPs have separate tiers.) The paid plans also include features like alerts, advanced reporting, API access, and even additional services like blacklist monitoring and email threat intelligence. In summary, EasyDMARC provides all the record-check tools gratis to attract users, and monetizes through tiered subscriptions for ongoing DMARC monitoring and email security management.
    

### DMARCLY (Dmarcly.com)

- **Tools Offered:** DMARCLY is another DMARC-focused service with a rich toolset for email admins. It provides **SPF, DKIM, and DMARC record generators** (wizards to create the correct DNS TXT entries) and **SPF, DKIM, DMARC record checkers** (to validate existing records)[dmarcly.com](https://dmarcly.com/tools/#:~:text=)[dmarcly.com](https://dmarcly.com/tools/#:~:text=). DMARCLY also offers a unique **SPF flattening tool** as part of its SPF checker – if you input your SPF record, it will resolve and “flatten” includes (useful for domains hitting the 10 DNS lookup limit)[dmarcly.com](https://dmarcly.com/tools/spf-record-checker#:~:text=Free%20SPF%20flattening%3F%20This%20tool,button%2C%20then%20scroll%20down). Additional tools include a combined **SPF/DKIM/DMARC configuration wizard** for setting up a domain from scratch[dmarcly.com](https://dmarcly.com/tools/#:~:text=), a **BIMI record generator and checker**[dmarcly.com](https://dmarcly.com/tools/#:~:text=), a basic **email blacklist lookup** for IP/domain[dmarcly.com](https://dmarcly.com/tools/#:~:text=), and an **MTA-STS/TLS-RPT checker** to validate those email security policies[dmarcly.com](https://dmarcly.com/tools/#:~:text=Blacklist%20Checker%20Check%20if%20your,domain%20or%20IP%20is%20blacklisted). These cover the full spectrum of email authentication and deliverability DNS records.
    
- **Free vs. Paid:** All the aforementioned DMARCLY tools are **free to use** on their website (they even allow getting a combined SPF/DKIM/DMARC analysis by simply sending an email to a specific address, which returns a report[dmarcly.com](https://dmarcly.com/tools/#:~:text=and%20if%20its%20syntax%20is,correct)). DMARCLY’s paid offering is its **DMARC report analyzer and monitoring platform**. They offer a 14-day free trial, then charge based on email volume monitored. (Specific pricing isn’t listed on the tools page, but DMARCLY positions itself similarly to EasyDMARC/dmarcian with tiered plans for DMARC compliance, generally starting with a basic plan and scaling up by volume of emails/domains monitored). In short, DMARCLY uses free tools (including the rare free SPF flattening service) as lead generation, and its paid service is ongoing DMARC monitoring and alerting.
    

### MXLayer (MXLayer.com)

- **Tools Offered:** MX Layer is an email security company that also provides a set of **free email tools** on its site. These tools include: **MX record lookup & mail server tester**, **SPF record checker** and **SPF generator**, **DKIM record checker**, **DMARC record lookup**, **IP address lookup**, **WHOIS lookup**, and **NS (nameserver) lookup**[mxlayer.com](https://www.mxlayer.com/email-tools/mx-lookup-checker-tool#:~:text=SPF%20Checker). The MX lookup tool not only retrieves MX records but also performs checks like testing the mail server’s response, checking for open relay, and verifying reverse DNS and response time[mxlayer.com](https://www.mxlayer.com/email-tools/mx-lookup-checker-tool#:~:text=Why%20Do%20You%20Need%20MX,Lookup%20and%20Record%20Checker%20Tool). All these utilities are accessible without needing an account. Essentially, MXLayer’s free toolkit overlaps a lot with DNSChecker and similar, focusing on DNS records and mail server integrity checks.
    
- **Free vs. Paid:** The tools listed above are **free**. MXLayer’s business is selling **managed email security services** (they offer inbound/outbound filtering, email archiving, compliance and continuity solutions). Their paid plans are organized in tiers: **Essentials at $25/month**, **Business Pro at $100/month**, and **Enterprise at $250/month** (monthly billing; slightly cheaper if paid annually)[mxlayer.com](https://www.mxlayer.com/business-email-security#:~:text=Billed%20%24250%20annually%20or%20%2425%2Fmo,billed%20monthly). These tiers correspond to increasing levels of email security features: e.g. the basic includes inbound gateway and spam filtering, the pro adds outbound filtering and data loss prevention, and Enterprise adds advanced features and API access[mxlayer.com](https://www.mxlayer.com/business-email-security#:~:text=Inbound%20Email%20Gateway%20Protection)[mxlayer.com](https://www.mxlayer.com/business-email-security#:~:text=Anti%20Virus%20%2F%20Anti%20Spam). The free tools are there to draw in administrators, whereas the **paid offerings are full security suite subscriptions**. There is also a 30-day free trial available for the security platform[mxlayer.com](https://www.mxlayer.com/email-tools/mx-lookup-checker-tool#:~:text=Special%20Offer%21). In summary, MXLayer’s lookup tools are free, and their revenue comes from selling cloud email security with multi-tier pricing.
    

### Mail-tester (mail-tester.com)

- **Tools Offered:** Mail-tester.com is a well-known tool to test email deliverability and spam score. The primary tool is the **Spam Test**: you send an email to a unique address they provide, then view a report showing your **SpamAssassin score**, checks for **SPF/DKIM/DMARC configuration**, whether your IP/domain is on major **blacklists**, and other diagnostics (like checking for proper MIME encoding, unsubscribe links, etc.). This is a single-use tool rather than lookups; it’s extremely useful for an email admin verifying if an outgoing email will pass filters. Mail-tester also provides an API for developers to integrate spam testing in their own apps and a feature to white-label the tests or embed the results in an iframe.
    
- **Free vs. Paid:** The basic mail-tester service (manual use through their website) is **free** for a limited number of tests per day (with some ads on the results page). For more frequent use or integration, they have a **credit-based pricing** model. Users can create an account and purchase test credits: for example, €50 for 500 tests, €80 for 1,000 tests, scaling up to larger bundles (up to 1,000,000 tests for €20,000)[mail-tester.com](https://www.mail-tester.com/manager/#:~:text=Number%20of%20tests%20Price%20%28excl,025%20%E2%82%AC%2015). There’s no expiration on purchased credits[mail-tester.com](https://www.mail-tester.com/manager/#:~:text=,want%20with%20the%20same%20account). They even offer a “micro-payment mode” for SaaS apps, where your end-users pay per test and you get a commission[mail-tester.com](https://www.mail-tester.com/manager/#:~:text=%2A%20Integrate%20mail,within%20the%20last%2030%20days). All advanced features (API access, result customization, history of tests, no ads, etc.) require using these paid credits. In short, **occasional testing is free via the web interface**[mail-tester.com](https://www.mail-tester.com/manager/#:~:text=If%20you%20occasionally%20perform%20manual,still%20use%20our%20web%20interface), but any heavy or automated use is paid via a flexible pay-as-you-go system.
    

_(Other notable mentions: **URIports** offers free validators for SPF, DKIM, DMARC, MTA-STS, etc., with a paid DMARC monitoring service[uriports.com](https://www.uriports.com/tools#:~:text=Validate%20your%20Web%20and%20Mail,setup)[uriports.com](https://www.uriports.com/tools#:~:text=Interested%20in%20URIports%20DMARC%20Monitoring%3F). **PowerDMARC** similarly provides free lookup/generator tools (SPF, DKIM, DMARC, BIMI)[powerdmarc.com](https://powerdmarc.com/mxtoolbox-alternatives/#:~:text=)[powerdmarc.com](https://powerdmarc.com/mxtoolbox-alternatives/#:~:text=) and sells a full email authentication SaaS. Historically **DNSstuff** provided many tools but has been retired as a free service. These services reinforce the trend: free diagnostics tools combined with a paid advanced service.)_

## Master List of Potential Email Admin Tools

Drawing from the above competitors, below is a master list of **potential tools** and features that could be included in a new “email admin toolbox” service. We’ve compiled all unique or common tools that email administrators find useful:

- **DNS Record Lookup Tools:** Basic DNS queries for records: **MX lookup**, **A/AAAA lookup**, **CNAME lookup**, **NS lookup**, **SOA record check**, **TXT record lookup** (for any text, including SPF/DMARC records). These are fundamental for checking how a domain is configured.
    
- **Email Authentication Tools:**
    
    - **SPF Record Checker** (validate syntax of SPF, check included domains, and show the resolved SPF mechanism tree)[dmarcly.com](https://dmarcly.com/tools/#:~:text=).
        
    - **SPF Record Generator** (wizard to create an SPF policy by selecting allowed senders/IPs)[dmarcly.com](https://dmarcly.com/tools/#:~:text=).
        
    - **SPF Flattening Tool** (resolve all “include” mechanisms into a flat list of IPs to avoid the 10-lookup limit[dmarcly.com](https://dmarcly.com/tools/spf-record-checker#:~:text=Free%20SPF%20flattening%3F%20This%20tool,button%2C%20then%20scroll%20down)).
        
    - **DKIM Record Checker** (verify a published DKIM public key for a given selector/domain)[dmarcly.com](https://dmarcly.com/tools/#:~:text=).
        
    - **DKIM Record Generator** (assist in formatting a DKIM TXT record given a public key and selector)[dmarcly.com](https://dmarcly.com/tools/#:~:text=).
        
    - **DMARC Record Checker** (validate DMARC TXT record syntax and policy)[dmarcly.com](https://dmarcly.com/tools/#:~:text=).
        
    - **DMARC Record Generator** (wizard to create a DMARC policy with desired policy/quarantine settings and report addresses)[dmarcly.com](https://dmarcly.com/tools/#:~:text=).
        
    - **BIMI Record Checker** (verify BIMI SVG record exists and is correctly formatted)[dmarcly.com](https://dmarcly.com/tools/#:~:text=).
        
    - **BIMI Record Generator** (help construct a BIMI DNS record linking to a logo)[dmarcly.com](https://dmarcly.com/tools/#:~:text=).
        
    - **MTA-STS Policy Checker** (check if a domain has a valid MTA-STS HTTPS policy and DNS record)[dmarcly.com](https://dmarcly.com/tools/#:~:text=Blacklist%20Checker%20Check%20if%20your,domain%20or%20IP%20is%20blacklisted).
        
    - **TLS-RPT Record Checker** (verify if SMTP TLS reporting is enabled correctly in DNS) – often combined with MTA-STS check[dmarcly.com](https://dmarcly.com/tools/#:~:text=Blacklist%20Checker%20Check%20if%20your,domain%20or%20IP%20is%20blacklisted).
        
- **Mail Server Tests:**
    
    - **MX Record Priority Test / Mail Flow Test** – verify that all listed MX servers are responding and see their priority order. Possibly connect to each MX to ensure it **accepts connections** and is not an open relay[mxlayer.com](https://www.mxlayer.com/email-tools/mx-lookup-checker-tool#:~:text=Why%20Do%20You%20Need%20MX,Lookup%20and%20Record%20Checker%20Tool).
        
    - **SMTP Connectivity Test** (given a domain, attempt SMTP handshake with its MX and report success/failure, supported TLS versions, certificate info – similar to CheckTLS’s core test).
        
    - **Open Relay Check** – test if an SMTP server will relay email from an external domain (to ensure it’s not an open relay, which is a vulnerability).
        
    - **Mail Server Banner Check** – retrieve the SMTP banner and verify it matches the domain or is not on a blacklist.
        
    - **SMTP TLS Checker** – detailed test of a mail server’s SSL/TLS: certificate valid? which protocols/ciphers supported? (This could integrate with the above connectivity test)[checktls.com](https://www.checktls.com/TestReceiver#:~:text=Because%20CheckTLS%20focuses%20on%20security%2C,with%20recording%20everything%2C%20it).
        
    - **Outbound SMTP Test** – perhaps allow the admin to send a test email through their SMTP and analyze the transaction (this is more complex, might be later feature).
        
- **Email Deliverability and Content Tools:**
    
    - **Blacklist Lookup (RBL Check)** – check if a mail server’s IP (or domain) is on common blacklists (Spamhaus, Barracuda, etc.)[hetrixtools.com](https://hetrixtools.com/mxtoolbox-alternative/#:~:text=checked%20every%207%20days%2C%20on,just%2030%20RBLs).
        
    - **Domain Reputation Check** – query services for sender reputation (e.g. Google Postmaster data, if accessible via API, or use a service’s rating if available).
        
    - **Spam Assassin Score Tester** – allow sending an email to get a spam filter score (like Mail-tester’s core function). This requires an email receiving setup; could be implemented similar to Mail-tester’s model.
        
    - **Inbox Placement Test (seed list test)** – (Likely a paid advanced tool) send an email to multiple test addresses at various ISPs and show where it lands (inbox/spam). This requires infrastructure and would be a premium feature (as GlockApps does).
        
    - **Email Header Analyzer** – parse a given email header to show the hops, delays, and any SPF/DKIM/DMARC authentication results or failures (both DNSChecker[dnschecker.org](https://dnschecker.org/all-tools.php#:~:text=Trace%20Email%20,the%20Location%20of%20Email%20Sender) and Google’s Messageheader tool do this). This helps diagnose why an email was marked spam or delayed.
        
- **Diagnostic & Misc Tools:**
    
    - **DNS Health Check** – an automated audit of a domain’s DNS for common errors (similar to IntoDNS), including checking for multiple MX, consistency of NS records, DNSSEC status, etc.[slashdot.org](https://slashdot.org/software/comparison/DNS-Check-vs-IntoDNS/#:~:text=Compare%20DNS%20Check%20vs,Additionally%2C%20it%20offers%20recommendations).
        
    - **Domain Scanner** – combine multiple checks (SPF, DKIM, DMARC, MX, blacklists, open ports) into one report for a domain (many services offer this as a one-click “domain health” report[easydmarc.com](https://easydmarc.com/tools/domain-scanner#:~:text=EasyDMARC%20Domain%20Scanner%20and%20Health,your%20domain%20in%20one%20click)).
        
    - **WHOIS Lookup** – retrieve domain whois info (useful for finding contacts or registrar info if diagnosing issues).
        
    - **DNS Propagation Checker** – check DNS records (like MX) across multiple global name servers to see if recent changes have propagated.
        
    - **SMTP Header (SPF/DKIM) Decoder** – input an email’s `Received-SPF` or `Authentication-Results` header to get a human-readable explanation (useful for complex DKIM failure reasons, etc.).
        
    - **Email/URL Safety Checker** – e.g., a tool to input a URL (found in an email) to see if it’s reported as phishing or in Google Safe Browsing (similar to EasyDMARC’s phishing link checker).
        
    - **API Access** – an API that allows all the above lookups and tests to be performed programmatically (for integration with other systems or automation).
        
    - **Monitoring/Alerts** – the ability to continuously monitor certain aspects: e.g., monitor if a domain’s DNS records change (DNS monitor), monitor if an IP gets blacklisted (blacklist monitor), monitor DMARC reports for failures, etc., with email alerts to the user. (This crosses into paid service territory, see below.)
        

This master list encompasses tools that are common across many competitors or particularly valuable to email administrators. A new email admin toolbox could start with some of these and expand over time.

## Recommendations for a New Email Admin Toolbox Service

Using the above research, here are recommendations on how to structure the offerings of a new service, including which tools to provide for free, which to reserve for paid tiers, pricing strategy, and a rollout plan:

### 1. **Free Tools to Attract Users**

Offer a robust set of **free tools** upfront to draw in email administrators. These free tools should cover the basics of setup and troubleshooting – essentially “give away” the quick lookups and one-off diagnostics that solve immediate problems. Based on competitor strategies, the following should be free:

- **DNS and Basic Email Record Lookups:** Free unlimited queries for MX records, A/NS records, CNAME, etc., and specifically SPF/DKIM/DMARC record checks and generators[dmarcly.com](https://dmarcly.com/tools/#:~:text=)[dmarcly.com](https://dmarcly.com/tools/#:~:text=). This matches what DNSChecker, EasyDMARC tools, Google, etc., do – these are table stakes that bring users to the site.
    
- **Email Authentication Utilities:** Free SPF/DKIM/DMARC generators and checkers (as above), plus BIMI checker, MTA-STS checker, and basic **domain health scan**[easydmarc.com](https://easydmarc.com/tools/domain-scanner#:~:text=EasyDMARC%20Domain%20Scanner%20and%20Health,your%20domain%20in%20one%20click). These are often provided free by DMARC services to entice sign-ups.
    
- **Basic Blacklist Check:** Allow users to perform a one-time blacklist lookup for an IP or domain for free[hetrixtools.com](https://hetrixtools.com/mxtoolbox-alternative/#:~:text=checked%20every%207%20days%2C%20on,just%2030%20RBLs). This addresses urgent “are we blacklisted?” questions without payment. (You can limit how many different IPs can be checked per day to prevent abuse, but a single-IP check should be free.)
    
- **SMTP Server Tests (Basic):** Provide a simple “test my mail server” connectivity check for free. For example, a test to connect to the domain’s primary MX and report if it responds and if TLS is available. This could be rate-limited but free, to help admins ensure their server is reachable. CheckTLS allows basic tests free, which gets admins interested[checktls.com](https://www.checktls.com/#:~:text=Authority%20www,a%20One%20Time%20List).
    
- **Email Header Analyzer:** A tool to paste an email header and get a parsed result should be free. This is very useful for troubleshooting and doesn’t cost much to implement (Google offers it free, and DNSChecker has one too). It draws in more users and keeps them on the site.
    
- **Limited Spam/Test Email:** Offer a **single-address mail-tester** function free. For instance, allow users to send an email to a provided address and get a spam score and basic analysis **a few times per day** without charge. This replicates mail-tester’s free usage mode, giving a taste of deliverability testing. It’s a strong hook for users trying to debug spam issues.
    

All these free tools build trust and demonstrate the service’s value. They should require minimal or no signup (perhaps allow anonymous use with CAPTCHA or require a free account for heavy use). The goal is to position the service as the go-to free toolbox for everyday email admin tasks, amassing a user base.

### 2. **Premium/Paid Tools and Features**

Reserve more resource-intensive, advanced, or ongoing-value features for the **paid tier(s)**. These are capabilities that go beyond one-off checks and provide continuous monitoring, higher volume, or deeper analysis – things an email admin would pay for once they see the benefit. Recommended paid tools/features:

- **High-Volume or Automated Usage:** The same lookups that are free via the web can be monetized via an API or bulk interface. For example, offer an **API access** or bulk query system as a paid feature for users who want to integrate these tools into their own scripts/software. Competitors like CheckTLS and mail-tester charge for API/automation[mail-tester.com](https://www.mail-tester.com/manager/#:~:text=%2A%20Integrate%20mail,daily%2C%20weekly%20and%20monthly%20usage)[checktls.com](https://www.checktls.com/SubscriptionSignUp#:~:text=%2F%2Fsubscription%2Fsignup%20,USD%29).
    
- **DMARC Aggregate Reporting and Monitoring:** This is a classic paid service – collect DMARC XML reports on behalf of users, provide a dashboard of sending sources, weekly summaries, alert on authentication failures, etc. Almost all DMARC-focused companies (EasyDMARC, DMARCLY, dmarcian) charge for this after a certain free limit. Your service can include a **free quota (e.g. process X emails/month)** and then require a paid plan for higher volumes[easydmarc.com](https://easydmarc.com/pricing/easysender/inbox-placement#:~:text=DMARC%20Report%20Reader%2C%20Analyzer%20,the%20best%20DMARC%20tools%2C).
    
- **Blacklist Monitoring (Continuous):** Provide the ability for a user to add, say, up to 5 IPs/domains to a monitor list on a paid plan. The system will check those IPs against dozens of RBLs every few hours and alert the user if listed. Free tier might only check daily or cover 1 IP; paid users get more IPs and faster checks[hetrixtools.com](https://hetrixtools.com/mxtoolbox-alternative/#:~:text=,days%2C%20on%20just%2030%20RBLs). This ongoing protection is worth paying for, as HetrixTools’ model shows.
    
- **Inbox Placement and Advanced Deliverability Tests:** If building these, they should be paid given the overhead. For instance, a feature like GlockApps’ seed list test (sending to many mailboxes across providers) or detailed spam filter analytics can be credit-based or included only in higher plans. These tools directly support marketers and high-stakes senders, who are willing to pay for improved deliverability insight.
    
- **Enhanced SMTP Testing:** A premium “security audit” test that does deep checks (e.g., tries sending an email with and without TLS, tests multiple cipher scenarios, verifies certificate details, checks all MX servers, etc.) can be part of a paid plan. Basic connectivity is free, but a thorough **compliance scan** (maybe outputting a report on how well the email server is configured, with grades) could be paid.
    
- **Monitoring & Alerts Suite:** Paid plans should unlock various monitors:
    
    - **DNS Change Monitor** – alerts if any DNS record on the domain changes (useful for security).
        
    - **MX/SMTP Monitor** – periodic test to ensure the mail server is up and not on any new blacklists (like a heartbeat).
        
    - **SSL Certificate Monitor** for the mail server’s TLS cert expiring.
        
    - **Email Round-Trip Monitor** – send a test email through their system to itself periodically to ensure mail flow works (advanced).  
        These kinds of ongoing services are valuable to businesses and can justify a subscription fee.
        
- **Volume Expansions and White-Labeling:** Following mail-tester’s example, allow organizations to purchase large blocks of spam tests or API queries. Offer white-label options (their branding on reports) on higher-tier plans. These are paid add-ons that cater to power users like ESPs or MSPs.
    

In summary, **anything that is continuous, bulk, or mission-critical should be paid**. Free solves immediate issues; paid ensures ongoing peace of mind and advanced insight.

### 3. **Suggested Pricing Tiers**

A multi-tier pricing model (freemium to higher tiers) is recommended to capture both small and large users. Based on competitor pricing and typical SaaS structures, consider the following tiers:

- **Free Tier:** $0 – Includes all the free tools (DNS lookups, SPF/DKIM/DMARC/BIMI checks, etc.) and perhaps **a very limited amount of monitoring** to give a taste (e.g., 1 domain on DMARC monitoring with up to 10k emails/month[easydmarc.com](https://easydmarc.com/pricing/easysender/inbox-placement#:~:text=DMARC%20Report%20Reader%2C%20Analyzer%20,the%20best%20DMARC%20tools%2C), 1 IP for blacklist monitor checked daily, 3 free spam tests/day, etc.). The free tier should require sign-up for monitoring features, so you can upsell later, but no credit card. This tier’s goal is maximizing user sign-ups.
    
- **Basic Tier:** (~$20–$30 per month range, or ~$200/year). This tier targets small businesses or individual consultants. It could include: **DMARC monitoring for a couple domains** (say 2 domains, up to 100k emails/month total)[g2.com](https://www.g2.com/products/easydmarc/pricing#:~:text=EasyDMARC%20Pricing%202025%20EasyDMARC%20Pricing,Data%20History%3B%201%20User), **Blacklist monitoring for perhaps 10–20 IPs/domains**, checks every 6–12 hours, **uptime/SMTP monitoring** for a couple of servers, and perhaps **a bundle of spam test credits** (e.g. 50 tests/month). It should also grant **API access** with moderate rate limits. Essentially, Basic covers one organization’s needs with a bit of everything (auth + monitoring). For pricing, note that EasyDMARC’s starter is ~$35.99[g2.com](https://www.g2.com/products/easydmarc/pricing#:~:text=EasyDMARC%20Pricing%202025%20EasyDMARC%20Pricing,Data%20History%3B%201%20User), and MXLayer’s lowest is $25[mxlayer.com](https://www.mxlayer.com/business-email-security#:~:text=Billed%20%24250%20annually%20or%20%2425%2Fmo,billed%20monthly)– around this range is palatable for small users.
    
- **Professional Tier:** (~$75–$100 per month, or ~$1000/year). This for medium businesses or heavy users. It would include higher limits: **multiple domains (5-10) for DMARC**, **higher DMARC report volume** (maybe up to 1 million emails/month), **50+ blacklist monitors** with checks every 1–4 hours, **multiple server monitors**, and **a larger pool of spam/inbox tests** (maybe 200/month). Also perhaps advanced features like **priority support**, **white-label reporting** (their logo on reports), and **customizable alerts**. This tier might also unlock the **Inbox Placement testing feature** (or give credits for it if it’s a separate usage-based component). GlockApps’ basic is $79 for 100k DMARC and 300 tests[g2.com](https://www.g2.com/products/glockapps/pricing#:~:text=Basic,Monitors%3B%201%20User%3B%20All), and their higher plan ~$129[subscribed.fyi](https://subscribed.fyi/glockapps/pricing/#:~:text=GlockApps%20Pricing%20and%20Features%20overview,Enterprise%20Plan%3A%20%24129%2Fmonth%2C) – so $100-ish for a robust plan seems reasonable.
    
- **Enterprise Tier:** (>$200/month, often custom pricing). Aimed at ESPs, large senders, or MSPs managing many client domains. This would include **very high limits or unlimited** DMARC reporting (10+ domains, millions of emails), **hundreds of blacklist monitors** (with checks as frequently as hourly or even on-demand), **API with high rate limits** for integration into their dashboards, **unlimited spam test usage** or deeply discounted credits, **dedicated support**, maybe **onboarding assistance**. Enterprise could also include add-ons like **Hosted BIMI SVG hosting**, **Managed DMARC enforcement consulting**, etc., depending on how far the service goes. Pricing could be tiered by number of domains or IPs beyond the base; e.g., $250/month for up to X domains, $500/month for larger, or purely custom quotes. (MXLayer’s top is $250/mo for their security product[mxlayer.com](https://www.mxlayer.com/business-email-security#:~:text=Get%20Started), and many deliverability SaaS go into several hundred for enterprise clients.)
    
- **Pay-as-you-go Options:** In addition to tiers, consider selling **credit packs** for specific tools (like mail-tester does). For example, if someone only wants the spam testing/inbox placement feature, allow them to buy a pack of 100 tests for $X without subscribing. This way, you monetize occasional users who may not need full monitoring. Similarly, API usage beyond a tier’s limit might be monetized per 1,000 calls, etc.
    

The above pricing should be regularly evaluated against competitors. Importantly, keep the **free tier truly useful but naturally limited** – enough that users see value and want more. For instance, free DMARC for one domain is great for a personal domain, but a business with 5 domains will immediately consider a paid plan.

### 4. **Need for Multi-Tiered Pricing**

A multi-tiered pricing approach **is likely necessary** and beneficial. The research indicates different competitors target different segments: some users will only use free tools occasionally, some will pay a little for basic monitoring, and larger organizations will pay more for comprehensive solutions. Having multiple tiers allows the service to capture value from all segments:

- **Free vs Paid Conversion:** Freemium is a common strategy here. MXToolbox (excluded, but known) and others give lots of free lookups but restrict monitoring to paid plans[hetrixtools.com](https://hetrixtools.com/mxtoolbox-alternative/#:~:text=,days%2C%20on%20just%2030%20RBLs). This funnel is effective – the free tier spreads awareness and the pain of its limits pushes power-users to paid. Without multiple tiers, you’d either give away too much (if one low price fits all unlimited) or turn away entry-level users (if only a high-priced plan exists). So at minimum, a Free – Basic – Pro structure is warranted.
    
- **Catering to Varied Needs:** Email admins range from a single domain admin to an MSP managing 100 domains. One price will not fit all. A small consultant might pay $20/month but not $100; an enterprise might need features beyond a $20 plan’s scope. Tiers allow scaling resources and features with willingness to pay. For example, CheckTLS has both $100/yr individual and $250/yr corporate options[checktls.com](https://www.checktls.com/SubscriptionSignUp#:~:text=%2F%2Fsubscription%2Fsignup%20,USD%29), EasyDMARC has Plus and Premium levels[g2.com](https://www.g2.com/products/easydmarc/pricing#:~:text=EasyDMARC%20Pricing%202025%20EasyDMARC%20Pricing,Data%20History%3B%201%20User), etc. This indicates multi-tier is standard in this space.
    
- **Add-On Flexibility:** Tiers also enable offering optional add-ons (like extra test credits or more monitors) on top of a base plan. This granular approach maximizes revenue from power users while keeping the entry barrier low for newcomers.
    

The only scenario to avoid multi-tier would be if the new service decided to focus narrowly (e.g., only DMARC monitoring) and price simply by volume. But since the vision is a **toolbox with many tool types**, multi-tier is the sensible route to bundle features appropriately.

Thus, implement at least three tiers (Free, mid, high) as described, with clear upgrade paths. This will align the service with competitor practices and user expectations.

### 5. **Launch Order and Development Priority**

To build the service effectively, we suggest a phased implementation of tools, prioritizing those that provide the most user value for the least complexity, especially focusing on free tools first to build traction:

- **Phase 1 – Core Free Tools Launch:** Start with the foundational lookup tools that are relatively straightforward to implement using existing DNS queries or libraries. This includes **MX Lookup, A/CNAME/NS lookups, SPF/DKIM/DMARC record checkers and generators, WHOIS lookup, basic blacklist check (single IP/domain), and the email header analyzer**. These are must-haves that will establish the site as useful. They also require minimal backend beyond DNS resolvers and some parsing logic. By launching these first (perhaps all at once or in quick succession), you populate the site with content and can start attracting users organically (SEO for “MX lookup” etc.). Ensure the UI is clean and results are easy to copy/share.
    
- **Phase 2 – Advanced Free Tools & Light Monitoring:** Next, add slightly more complex free offerings like the **SMTP connectivity test** (which involves making an actual connection to mail servers) and the **SPF flattening tool** (which requires recursive DNS resolution logic) since these differentiate the service. Also introduce user accounts and allow a **taste of monitoring** in this phase: for instance, users can sign up to save a DMARC report receiver address and see a sample DMARC dashboard for one domain, or set one IP on daily blacklist check. This phase gets users logging in and provides a bridge to paid features. It’s also the time to gather feedback and ensure accuracy of all the free tool results.
    
- **Phase 3 – Paid Feature Rollout (Basic Tier):** Implement the initial paid offerings focusing on the most demanded monitoring tasks. For example, roll out **DMARC aggregate monitoring** fully (with parsing, charts, alerts), and **Blacklist Monitoring** for multiple IPs with alerts. These two are likely to convert many free users to paid, as they address ongoing needs. Also release the **API access** around this time for those who want automation (could be tied to the Basic tier or as a separate developer plan). Pricing for the Basic tier can be announced at this point and should be attractive. The goal in this phase is to turn active free users into paying customers by solving daily pain points (catching when their domain is abused or an IP gets blacklisted promptly).
    
- **Phase 4 – Expanded Paid Tools (Pro Tier and above):** Now add the more sophisticated and resource-intensive tools for higher-tier plans. This includes **inbox placement testing** (developing a seed list system or partnering to get one), **automated spam scoring for emails** (possibly in-house using SpamAssassin or via integration), and expanded monitoring like **uptime/SMTP round-trip monitoring**, **Hosted MTA-STS (with certificate hosting)**, etc. Also build out quality-of-life features for pro users: e.g. **PDF reporting, team accounts, role-based access** if multiple team members will use the service (common in enterprise plans). Launch a Pro tier with these features bundled. Multi-tier pricing (Pro, Enterprise) becomes available. This phase targets power-users and larger organizations, bringing in higher MRR per customer.
    
- **Phase 5 – Continuous Improvement and Add-ons:** After core offerings are stable, iterate with additional tools or add-ons based on user requests. For instance, add a **phishing URL scanner**, a **security.txt record checker** (as URIports does), or a **comprehensive domain security audit report**combining all checks into one PDF. Also consider building a **community knowledge base** or automated “fix suggestions” in tool outputs (many tools already give suggestions, e.g., IntoDNS references RFCs for fixes[intodns.com](https://intodns.com/#:~:text=intoDNS%3A%20checks%20DNS%20and%20mail,fix%20and%20improve%20them%2C)). These improvements will help retain users and upsell them to higher plans over time. At this stage, you can also explore partnerships (e.g., integrate data from Spamhaus, or offer a credit system for sending test emails via a third-party if needed).
    

In terms of **launch order of specific tools**, it would look like:

1. **MX/SPF/DKIM/DMARC lookup tools (free)** – Day 1 offering.
    
2. **DNS & WHOIS lookups (free)** – Day 1.
    
3. **Basic blacklist single-check (free)** – Day 1.
    
4. **Email header analyzer (free)** – Day 1 or soon after.
    
5. **SMTP tester (free)** – shortly after launch, once basic tools are stable (since this requires more coding/testing).
    
6. **User accounts & sample monitoring (free limited)** – Phase 2.
    
7. **SPF flatten tool (free)** – Phase 2 (to generate buzz as a unique freebie).
    
8. **DMARC monitoring (paid)** – Phase 3 priority (high demand feature)[easydmarc.com](https://easydmarc.com/pricing/easysender/inbox-placement#:~:text=Start%20reading%20and%20analyzing%20Your,the%20best%20DMARC%20tools%2C).
    
9. **Blacklist monitoring (paid)** – Phase 3.
    
10. **API access (paid)** – Phase 3.
    
11. **Advanced deliverability tests (paid)** – Phase 4 (inbox placement, content scoring).
    
12. **Additional monitors and integrations (paid)** – Phase 4 (uptime, MTA-STS hosting, etc.).
    
13. **Misc niche tools** – Phase 5 as needed (e.g., advanced DNSSEC checks, ARC validation tool, etc., if there’s user interest).
    

By launching free tools first (especially those overlapping what DNSChecker, Google Toolbox, etc. offer), you capture user attention and trust. Then by rolling out the most requested paid features in stages, you keep users engaged and have multiple opportunities to market new capabilities. A phased approach also helps to not overwhelm development and to gather user feedback at each stage, ensuring the service’s tools are accurate and user-friendly.

---

By following this plan – offering a rich free toolbox and layering paid services for power users – the new email admin toolbox can penetrate the market quickly and grow into a sustainable service. The combination of essential free utilities and valuable paid monitoring aligns with what successful competitors are doing, leveraging the best of both worlds: community goodwill and commercial success. Each tool and feature listed above has proven demand in the industry, and with thoughtful implementation and pricing, the new service can become an indispensable resource for email administrators.

**Sources:**

- DNSChecker free DNS & email tools[dnschecker.org](https://dnschecker.org/dkim-record-checker.php#:~:text=Related%20tools%20DMARC%20Generator%20,Record%20Check%20%20%2011)[dnschecker.org](https://dnschecker.org/mx-lookup.php#:~:text=MX%20Lookup%20,check%20their%20mail%20exchange%20records)
    
- IntoDNS domain & mail server report (free)[intodns.com](https://intodns.com/#:~:text=intoDNS%3A%20checks%20DNS%20and%20mail,fix%20and%20improve%20them%2C)[intodns.io](https://intodns.io/api-docs/v1/#:~:text=Usage%20limits%20and%20terms%20of,use%20and%20disclaimer)
    
- Google Admin Toolbox (free SPF/DKIM/DMARC checks)[dmarcreport.com](https://dmarcreport.com/blog/learning-to-setup-spf-dkim-and-dmarc-in-google-workspace/#:~:text=Learning%20to%20Setup%20SPF%2C%20DKIM%2C,Although)[support.duda.co](https://support.duda.co/hc/en-us/articles/26649783401367-Google-s-Email-Sender-Guidelines#:~:text=Google%27s%20Email%20Sender%20Guidelines%20,Up%20DKIM%20TXT%20Records)
    
- CheckTLS SMTP test tools and pricing[checktls.com](https://www.checktls.com/aboutsubscription.html#:~:text=,Payment)[checktls.com](https://www.checktls.com/SubscriptionSignUp#:~:text=%2F%2Fsubscription%2Fsignup%20,USD%29)
    
- HetrixTools blacklist monitor free vs. paid[hetrixtools.com](https://hetrixtools.com/#:~:text=15%20Uptime%20Monitors%20%E2%80%A2%201,keep%20it%20clean%20from%20spam)[hetrixtools.com](https://hetrixtools.com/mxtoolbox-alternative/#:~:text=,0388%2FIP%2Fmonth)
    
- GlockApps tools and pricing tiers[glockapps.com](https://glockapps.com/dmarc-analyzer/#:~:text=DMARC%20Checker%20,with%20GlockApps%20DMARC%20Analyzer%20Tool)[g2.com](https://www.g2.com/products/glockapps/pricing#:~:text=Basic,Monitors%3B%201%20User%3B%20All)
    
- EasyDMARC tools and pricing[easydmarc.com](https://easydmarc.com/tools#:~:text=Free%20tools%3B%20Domain%20Scanner%20%C2%B7,Record%20Checker%20%C2%B7%20All%20tools)[easydmarc.com](https://easydmarc.com/pricing/easysender/inbox-placement#:~:text=DMARC%20Report%20Reader%2C%20Analyzer%20,the%20best%20DMARC%20tools%2C)
    
- DMARCLY tools (SPF/DKIM/DMARC/BIMI, free)[dmarcly.com](https://dmarcly.com/tools/#:~:text=)[dmarcly.com](https://dmarcly.com/tools/#:~:text=)
    
- MXLayer free tools and security suite pricing[mxlayer.com](https://www.mxlayer.com/email-tools/mx-lookup-checker-tool#:~:text=SPF%20Checker)[mxlayer.com](https://www.mxlayer.com/business-email-security#:~:text=Billed%20%24250%20annually%20or%20%2425%2Fmo,billed%20monthly)
    
- Mail-tester free usage and credit pricing[mail-tester.com](https://www.mail-tester.com/manager/#:~:text=Number%20of%20tests%20Price%20%28excl,025%20%E2%82%AC%2015)[mail-tester.com](https://www.mail-tester.com/manager/#:~:text=If%20you%20occasionally%20perform%20manual,still%20use%20our%20web%20interface)
    

![Favicon](https://www.google.com/s2/favicons?domain=https://support.duda.co&sz=32)

![Favicon](https://www.google.com/s2/favicons?domain=https://dmarcreport.com&sz=32)

![Favicon](https://www.google.com/s2/favicons?domain=https://intodns.io&sz=32)

![Favicon](https://www.google.com/s2/favicons?domain=https://intodns.com&sz=32)

![Favicon](https://www.google.com/s2/favicons?domain=https://dnschecker.org&sz=32)

Sources





# **Market Research: Continuously-Managed SPF Flattening Services**

## Introduction

Sender Policy Framework (SPF) flattening services help organizations overcome the SPF 10-DNS-lookup limit by converting all nested includes (and other lookup mechanisms) into a single, expanded set of IP addresses. Modern SPF flattening tools go further by **dynamically managing** the SPF records: they provide a stable DNS record (often via a third-party domain include) that automatically stays up-to-date as email service providers add or change IP ranges. This “set-and-forget” approach ensures you remain under the lookup limit without constant manual updates[dmarcreport.com](https://dmarcreport.com/flatten-spf-files-to-ensure-email-deliverability/#:~:text=AutoSPF%20automatically%20flattens%20SPF%20records%2C,process%20is%20highly%20automated%2C%20inherently)[autospf.com](https://autospf.com/#:~:text=We%20automate%20regular%20checks%20for,comes%20to%20your%20technology%20stack). Below, we survey the existing market of such SPF flattening services, detailing their delivery methods, pricing models, update frequency, and how they are offered (stand-alone vs. part of larger email tool suites).

## **1. Competitors Offering SPF Flattening Services**

### **AutoSPF (Standalone SPF Flattening Service)**

**Offering & Delivery:** AutoSPF is a dedicated, stand-alone SPF flattening service (not part of a broader DMARC suite) focusing **100% on SPF management**[autospf.com](https://autospf.com/#:~:text=100). It provides a single include record for your domain’s SPF which consolidates all nested includes. Setup is simple: you “copy, paste, replace” your old SPF with the AutoSPF include, and their system handles the rest[autospf.com](https://autospf.com/#:~:text=,of%20additional%20networks%20and%20senders)[autospf.com](https://autospf.com/#:~:text=We%20automate%20regular%20checks%20for,comes%20to%20your%20technology%20stack). AutoSPF hosts the flattened SPF records on Cloudflare-backed DNS for high availability[autospf.com](https://autospf.com/#:~:text=with%20an%20automatically%20managed%20SPF,availability%20is%20ensured%20by%20Cloudflare). It continuously monitors your vendors’ SPF records and automatically updates the include record **every 5 minutes** to reflect any IP changes[autospf.com](https://autospf.com/#:~:text=We%20automate%20regular%20checks%20for,comes%20to%20your%20technology%20stack). This real-time updating guarantees that even if an email service provider adds new IPs, your SPF stays current without manual intervention.

**Pricing:** AutoSPF markets itself to businesses and enterprises, offering tiered pricing by number of domains. The service is **unlimited in email volume** (no cap on messages) and charges primarily per domain. As of recent data, AutoSPF’s **Plus plan** is about **$37/month for 1 domain**, **Premium $97/month for up to 5 domains**, and **Enterprise $387/month for up to 10 domains**[autospf.com](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=,ideal%20for%20enterprise%20organizations%3A%20%24387%2Fmonth). These plans include all features (continuous monitoring, real-time flattening, support for teams/multiple users, etc.). AutoSPF emphasizes “transparent pricing” with no per-email or per-user fees[autospf.com](https://autospf.com/#:~:text=No%20email%20sending%20limits%2C%20no,based%20pricing), though exact prices above a certain scale require contacting sales. A free trial or free basic tier may be available (AutoSPF documentation implies one can flatten “in one click and for free” on a trial basis[reddit.com](https://www.reddit.com/r/sysadmin/comments/m1f17s/spf_flattening_recommendations/#:~:text=There%20are%20two%20types%20of,flattening%20service)), but ongoing automated service is a paid offering.

**Update Frequency & Monitoring:** AutoSPF runs active checks for any changes in included SPF records **every 5 minutes**, one of the fastest intervals in the industry[autospf.com](https://autospf.com/#:~:text=We%20automate%20regular%20checks%20for,comes%20to%20your%20technology%20stack). This active polling means updates propagate extremely quickly (with a 60-second fix guarantee for broken records)[autospf.com](https://autospf.com/#:~:text=availability%20is%20ensured%20by%20Cloudflare)[autospf.com](https://autospf.com/#:~:text=%2A%20White,free%20%E2%80%93%20on%20us%21). The service is truly “set and forget,” requiring no regular logins or manual updates after initial setup[autospf.com](https://autospf.com/#:~:text=Set%20it%20up%20once%2C%20without,the%20need%20for%20regular%20logins).

**Integrations & APIs:** As a specialized tool, AutoSPF supports multi-domain management and team access (enterprise features include user management, SSO/SAML integration, SOC2 compliance, etc. for MSP or large org use)[autospf.com](https://autospf.com/#:~:text=,Vendor%20Security%20Questionnare). While specific API access isn’t publicly detailed, its focus on MSPs and enterprise suggests that integration capabilities (or at least an admin portal for managing customer domains) are available. The service’s sole purpose is SPF flattening, so it does not bundle unrelated features – it can be used alongside other email security tools if needed, but it remains a separate, standalone solution[autospf.com](https://autospf.com/#:~:text=No%20Extra%20Suite%20of%20Tools,Needed).

### **DMARCLY Safe SPF (Flattening as Part of DMARC Suite)**

**Offering & Delivery:** DMARCLY is a full DMARC monitoring and email security platform, and **“Safe SPF”** is DMARCLY’s built-in automated SPF flattening feature[dmarcly.com](https://dmarcly.com/blog/what-is-automatic-dynamic-spf-record-flattening-and-how-it-fixes-the-spf-permerror-too-many-dns-lookups-issue#:~:text=DMARCLY%27s%20Safe%20SPF%20feature%20implements,the%20DNS%20settings%20manually%20anymore). It is not sold as a separate product; instead, it’s included in DMARCLY’s subscription plans. Users set up Safe SPF via the DMARCLY dashboard (DNS Records -> Safe SPF) and are provided with a DNS record to publish. Typically, this involves adding **a single include under DMARCLY’s domain** (or a CNAME) in your SPF record – once this is in place, DMARCLY generates and maintains the flattened SPF behind that include[dmarcly.com](https://dmarcly.com/blog/what-is-automatic-dynamic-spf-record-flattening-and-how-it-fixes-the-spf-permerror-too-many-dns-lookups-issue#:~:text=final%20SPF%20record). Any changes in your third-party senders’ records are **automatically detected and updated** in the “Safe SPF” record, so your domain’s SPF is always current[dmarcly.com](https://dmarcly.com/blog/what-is-automatic-dynamic-spf-record-flattening-and-how-it-fixes-the-spf-permerror-too-many-dns-lookups-issue#:~:text=Fortunately%2C%20SPF%20record%20flattening%20can,to%20the%20final%20SPF%20record). In practice, once Safe SPF is activated, administrators no longer need to manually edit their SPF TXT; DMARCLY keeps it under the 10-lookup limit dynamically.

**Pricing:** Safe SPF is **included in DMARCLY’s paid plans** – there is _no standalone Safe SPF subscription_. DMARCLY’s **Professional plan (~$17.99/mo)** includes 1 Safe SPF domain, the **Growth plan (~$39.99/mo)**includes 2 Safe SPF domains, and higher tiers include more (Business ~$69.99 for 3-4 domains, Enterprise ~$199/mo for 4 Safe SPF domains)[domainadmintools.com](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=you%20to%20manage%20unlimited%20domains,cheaper%20if%20you%20go%20yearly)[dmarcly.com](https://dmarcly.com/pricing#:~:text=,DMARC%20compliant%20messages%20per%20month). If a customer needs to flatten additional domains beyond their plan’s quota, DMARCLY simply charges an overage fee of **$10 per extra domain** using Safe SPF[dmarcly.com](https://dmarcly.com/pricing#:~:text=,domains%20exceeds%20the%20plan%20quota). This effectively values the dynamic SPF service at $10/domain/month in their pricing model beyond the bundled allotment. (Notably, DMARCLY does **not** offer a free plan; they provide a 14-day trial, but ongoing use of Safe SPF requires a paid subscription[domainadmintools.com](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=match%20at%20L41%20Dmarcly%20does,Enterprise%20at%20%24199%20per%20month).)

**Update Frequency & Monitoring:** DMARCLY hasn’t published an exact polling interval, but it describes Safe SPF as _automatic/dynamic flattening happening every few minutes_[dmarcly.com](https://dmarcly.com/blog/what-is-automatic-dynamic-spf-record-flattening-and-how-it-fixes-the-spf-permerror-too-many-dns-lookups-issue#:~:text=Fortunately%2C%20SPF%20record%20flattening%20can,to%20the%20final%20SPF%20record). In other words, the system frequently (likely on the order of minutes or hourly) checks all included sender records. Any underlying change in a vendor’s SPF (new IP address, new include, etc.) will “make it to the final SPF record” automatically[dmarcly.com](https://dmarcly.com/blog/what-is-automatic-dynamic-spf-record-flattening-and-how-it-fixes-the-spf-permerror-too-many-dns-lookups-issue#:~:text=Fortunately%2C%20SPF%20record%20flattening%20can,to%20the%20final%20SPF%20record). This proactive monitoring ensures the flattened record is continuously up-to-date. Essentially, **whenever a third-party provider updates their SPF, DMARCLY’s system detects it and updates the DNS include record for you**, so you remain under the limit with no lapses.

**Integrations:** Since Safe SPF is part of a larger platform, users benefit from integration with DMARCLY’s DMARC analytics and UI. No direct API for just the SPF feature is advertised, but the platform approach means you manage SPF flattening alongside DMARC reporting, BIMI, MTA-STS, etc., in one dashboard[dmarcly.com](https://dmarcly.com/blog/what-is-automatic-dynamic-spf-record-flattening-and-how-it-fixes-the-spf-permerror-too-many-dns-lookups-issue#:~:text=DMARCLY%27s%20Safe%20SPF%20feature%20implements,the%20DNS%20settings%20manually%20anymore). Safe SPF is **bundled** – you cannot purchase it separately from DMARCLY’s suite. The target audience is those who are already investing in DMARC management and want an easy SPF solution included.

### **EasyDMARC EasySPF (Flattening as Part of DMARC Suite)**

**Offering & Delivery:** EasyDMARC, another popular email security SaaS, provides an automated SPF flattening feature called **“EasySPF.”** Like DMARCLY, this is integrated into their platform rather than a separate service. EasySPF works by having the user **add a single include** to their SPF record (pointing to EasyDMARC’s system), after which “EasyDMARC takes care of the rest”[easydmarc.com](https://easydmarc.com/easy-spf-too-many-dns-lookups#:~:text=Add%20a%C2%A0single%20include%C2%A0in%20your%20SPF,and%20EasyDMARC%C2%A0takes%20care%C2%A0of%20the%20rest). Essentially, you perform a one-time DNS setup and then manage your SPF through EasyDMARC’s web UI. Behind the scenes, EasySPF uses a **dynamic flattening algorithm that swaps out includes for IPs**, and it continuously **detects and applies changes** from your providers to keep the SPF current[easydmarc.com](https://easydmarc.com/tools/easy-spf#:~:text=management%20and%20results%20in%20an,immediate%20protocol%20failure). The benefit is you no longer need to edit DNS every time – any new sender or any vendor IP update is handled in the EasyDMARC platform and auto-reflected in DNS via that include record[easydmarc.com](https://easydmarc.com/tools/easy-spf#:~:text=EasySPF%20eliminates%20the%20constant%20back,automatically%20depicted%20in%20the%20DNS)[easydmarc.com](https://easydmarc.com/tools/easy-spf#:~:text=EasySPF%20uses%20a%20dynamic%20SPF,record%20is%20up%20to%20date). This dramatically simplifies SPF upkeep and prevents “permerror” issues caused by lookup limits[easydmarc.com](https://easydmarc.com/easy-spf-too-many-dns-lookups#:~:text=its%20own%20email%20authentication%20configurations,action%20to%20solve%20the%20issue)[easydmarc.com](https://easydmarc.com/easy-spf-too-many-dns-lookups#:~:text=,changed%20over%20time).

**Pricing:** EasyDMARC stands out for offering a **Free tier**, which is attractive to individuals or small organizations. Their free plan (great for “personal domains or small businesses”) includes basic DMARC reporting and apparently allows unlimited domain monitoring, but with limited features[domainadmintools.com](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=Easydmarc%20has%20a%20FREE%20plan,cheaper%20if%20you%20go%20yearly). It’s unclear if the free tier includes the dynamic EasySPF feature – likely it does _not_, or perhaps allows a basic flatten once, since EasySPF is a premium feature in most comparable services. Paid plans start at **$17.99/month (Plus)** and **$39.99/month (Premium)**, which align with DMARCLY’s pricing[domainadmintools.com](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=Easydmarc%20has%20a%20FREE%20plan,cheaper%20if%20you%20go%20yearly). According to one comparison, EasyDMARC’s Premium plan (around $39/mo) supports up to 4 domains and includes SPF flattening[domainadmintools.com](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=Price%20%28per%20month%29%2439,history%206%20months%201%20year). In fact, both EasyDMARC and DMARCLY include automated SPF flattening in these mid-tier plans[domainadmintools.com](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=Price%20%28per%20month%29%2439,history%206%20months%201%20year). EasyDMARC’s pricing is typically based on DMARC email volume and features, but **any paid plan (Plus and up) includes the EasySPF capability** at no extra cost. This means for roughly ~$18-$40/month, a small business gets DMARC monitoring plus managed SPF for a couple of domains. EasyDMARC also provides discounts for annual commitments (e.g. Premium at ~$35.99/mo on annual plan)[domainadmintools.com](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=Price%20%28per%20month%29%2439,history%206%20months%201%20year).

**Update Frequency & Monitoring:** EasySPF’s operation is fully automated and **real-time in effect**. The platform mentions that it “detects and applies changes” for your SPF record automatically[easydmarc.com](https://easydmarc.com/tools/easy-spf#:~:text=management%20and%20results%20in%20an,immediate%20protocol%20failure). While a precise interval isn’t stated, EasyDMARC likely updates records very promptly (possibly on a schedule or triggered by DNS TTLs of included records). The user doesn’t need to worry about checking for vendor updates – EasySPF continuously monitors the includes you’ve configured. In practice, once you’ve checked off which services you use (or imported your current SPF), any new IP ranges published by, say, Microsoft 365 or Mailchimp are picked up and your EasySPF record is updated behind the scenes.

**Integrations:** EasyDMARC’s solution does not require giving them direct DNS access; instead, **you point a DNS record to their infrastructure** (via include or sometimes CNAME for DMARC) and they handle changes[support.openprovider.eu](https://support.openprovider.eu/hc/en-us/articles/5294664442386--EasyDMARC-Frequently-Asked-Questions#:~:text=We%20strongly%20recommend%20using%20CNAME,DNS%20zone%20settings%20every%20time)[easydmarc.com](https://easydmarc.com/easy-spf-too-many-dns-lookups#:~:text=Add%20a%C2%A0single%20include%C2%A0in%20your%20SPF,and%20EasyDMARC%C2%A0takes%20care%C2%A0of%20the%20rest). This means EasySPF is easy to integrate – just a DNS include. The management is through their cloud dashboard (no standalone API for SPF alone, but the company does offer an API for its services in higher plans or for MSPs). EasyDMARC is also offered through partners (e.g., Openprovider resells it, instructing users to add a CNAME for DMARC and likely includes for SPF)[support.openprovider.eu](https://support.openprovider.eu/hc/en-us/articles/5294664442386--EasyDMARC-Frequently-Asked-Questions#:~:text=We%20strongly%20recommend%20using%20CNAME,DNS%20zone%20settings%20every%20time). In summary, EasySPF is **bundled with EasyDMARC’s broader email security platform** – you get it as part of a holistic package (DMARC, DKIM tools, reporting, etc.), and it’s not sold separately.

### **PowerDMARC “PowerSPF” (Flattening as Part of DMARC Suite)**

**Offering & Delivery:** PowerDMARC is a comprehensive email authentication SaaS (offering DMARC, DKIM, BIMI, etc.), and it provides an SPF flattening service branded as **PowerSPF**. This is a **“one-click automatic SPF flattening tool”** within their platform[powerdmarc.com](https://powerdmarc.com/spf-rate-limit-permerror/#:~:text=Your%20Dynamic%20and%20Hassle,Flattening%20tool). Similar to others, PowerSPF lets you flatten SPF records for multiple domains into a single include. The user interface likely allows you to select which sending services you use (or you input your current SPF), and then PowerSPF generates an optimized SPF record. The result is typically an include like `include:<_someid_.spfs.powerdmarc.com>` or similar, which you put in your DNS. PowerDMARC’s system then **resolves and compresses all the includes to IPs** and ensures the published record stays under 10 lookups[powerdmarc.com](https://powerdmarc.com/spf-rate-limit-permerror/#:~:text=How%20Our%20SPF%20Flattening%20Tool,Works). They advertise that this happens **instantly and dynamically**, keeping records updated with any changes to authorized IPs[autospf.com](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=PowerDMARC%20is%20a%20SaaS%20platform,changes%20made%20to%20authorized%20IPs). In short, PowerSPF removes the need for multiple includes by replacing them with IP ranges, and it actively maintains that list over time.

**Pricing:** PowerDMARC’s flattening is included in its overall service packages. They offer a range of plans including a **Free plan ($0)** for personal/small use and a **Basic plan starting at $8/month** for businesses[autospf.com](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=,Enterprise%20Plan%3A%20Quote%20on%20Request). (The free tier typically allows 1 domain and up to 10,000 DMARC-compliant emails, while the $8/month Basic might cover ~5 domains and up to 2M emails[softwaresuggest.com](https://www.softwaresuggest.com/powerdmarc#:~:text=PowerDMARC%20Pricing%2C%20Features%2C%20and%20Reviews,%C2%B7%20Up%20to%202%2C000%2C000).) The key point is that even the low-tier paid plans include full SPF flattening capabilities. In fact, PowerDMARC highlights **PowerSPF** as part of its platform – you don’t pay per domain separately for flattening; it’s bundled. For larger needs, they have Enterprise and MSP plans by quotation[autospf.com](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=,MSP%2FMSSP%3A%20Quote%20on%20Request). For example, an organization could use the Basic $8/mo plan and get dynamic SPF for perhaps a couple domains, which is very cost-effective (though that plan also limits DMARC aggregate volume). **In competitive context, PowerDMARC is one of the most affordable options to get an automated SPF service**, thanks to its low entry price and even a free tier for one domain[autospf.com](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=,Enterprise%20Plan%3A%20Quote%20on%20Request). This indicates they treat SPF flattening as a value-added feature to attract users to their platform.

**Update Frequency & Monitoring:** PowerSPF is described as _dynamic_ and “staying updated on uninformed changes to authorized IPs”[powerdmarc.com](https://powerdmarc.com/spf-rate-limit-permerror/#:~:text=Your%20Dynamic%20and%20Hassle,Flattening%20tool). It likely checks the DNS of included senders frequently (possibly honoring the TTLs of those records, as one OnDMARC article suggests for dynamic SPF)[knowledge.ondmarc.redsift.com](https://knowledge.ondmarc.redsift.com/en/articles/3822217-how-does-dynamic-spf-work#:~:text=How%20does%20Dynamic%20SPF%20work%3F,includes%20honouring%20their%20published%20TTL). While exact timing isn’t given, PowerDMARC’s messaging suggests real-time or near-real-time updates. They note you can “stay under the lookup limit at all times” and easily accommodate new vendors[autospf.com](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=What%20to%20Expect%3F). In practice, once configured, the user’s SPF record remains valid continuously, as PowerDMARC’s cloud will update the IP lists whenever providers change (this might be done within minutes of a change, or at worst daily – the user doesn’t have to monitor it).

**Integrations:** Being part of a larger suite, PowerDMARC offers a web portal and API for its services (useful for MSPs or large enterprise workflows). The SPF tool is accessible via the same platform where you manage DMARC, etc. For integration, you just need to use the provided include in your DNS – no need to delegate full DNS control. PowerDMARC’s focus on MSP/MSSP (they have an MSP program)[autospf.com](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=,business%20owners%20and%20startups%3A%20%248%2Fmonth) implies multi-tenant management of flattening across many client domains is supported. In summary, **PowerSPF is bundled**with the broader PowerDMARC service; it is not sold as a standalone product. Customers typically come for a full email authentication solution, with SPF flattening as one feature (often called out as a differentiator).

### **MxToolbox SPF Flattening (Part of Email Delivery Suite)**

**Offering & Delivery:** MxToolbox, known for its email infrastructure tools, provides an SPF flattening service as a feature of its **Delivery Center** suite. Specifically, **Delivery Center Plus** (MxToolbox’s top-tier plan) includes **automatic SPF record flattening**[mxtoolbox.com](https://mxtoolbox.com/c/products/deliverycenterplus?source=FEAT-SPFF#:~:text=Mxtoolbox%20Delivery%20Center%20Plus%20has,for%20up%20to%205%20Domains)[mxtoolbox.com](https://mxtoolbox.com/c/products/deliverycenterplus?source=FEAT-SPFF#:~:text=great%20features%20of%20Delivery%20Center%2C,for%20up%20to%205%20Domains). The service works by **rewriting and hosting your SPF record** on MxToolbox infrastructure. Users input or verify their authorized senders (via the MxToolbox UI) and MxToolbox generates a flattened SPF record. According to their description, this “SPF Flattening Tool” reconstructs the SPF by expanding all includes to IPs and removing excess lookups[mxtoolbox.com](https://mxtoolbox.com/c/products/features/spf-flattening#:~:text=MxToolbox%20SPF%20Flattening%20Tool%20provides,reconstructing%20your%20SPF%20record%20automatically)[mxtoolbox.com](https://mxtoolbox.com/c/products/features/spf-flattening#:~:text=Image%3A%20SPF%20Flattening%20Simplifies%20the,SPF%20Record). MxToolbox likely provides either a hostname to include (e.g., `include:_<customer>._spf.mxtoolbox.com`) or instructs the user to publish a provided TXT record. Given the “set and forget” framing, it’s probable they host a special include record that you add once, after which any changes you make in their portal (like adding a new email service) are reflected in that hosted SPF entry[easydmarc.com](https://easydmarc.com/tools/easy-spf#:~:text=EasySPF%20eliminates%20the%20constant%20back,automatically%20depicted%20in%20the%20DNS)[easydmarc.com](https://easydmarc.com/tools/easy-spf#:~:text=EasySPF%20uses%20a%20dynamic%20SPF,record%20is%20up%20to%20date). This spares you from manually updating DNS each time a change is needed.

**Pricing:** Unlike others, MxToolbox **does not offer SPF flattening as a standalone purchase or in lower tiers**. It is bundled in their **Delivery Center Plus plan at $399/month**, which covers up to 5 domains and up to 5,000,000 emails/month[mxtoolbox.com](https://mxtoolbox.com/c/products/deliverycenterplus?source=FEAT-SPFF#:~:text=great%20features%20of%20Delivery%20Center%2C,for%20up%20to%205%20Domains)[mxtoolbox.com](https://mxtoolbox.com/c/products/deliverycenterplus?source=FEAT-SPFF#:~:text=,Domain%28s%29%3A%205). The standard Delivery Center (without “Plus”) at $129/month does **not** include flattening. Essentially, MxToolbox positions SPF flattening as a premium add-on feature for customers who subscribe to their highest-tier email deliverability solution. This high price is justified by the bundle of advanced tools that come with it (inbox placement, blacklist monitoring, etc.), but it means the **cost of entry for MxToolbox’s SPF flattening is very steep** compared to other solutions. There is no free or low-cost option for a single domain; it’s targeted at enterprise users who need a comprehensive deliverability suite. (For reference, MxToolbox’s solution at $399/mo for 5 domains averages to ~$80 per domain monthly, far above the market rate if one only needs SPF flattening.) They encourage interested users to **“Sign Up” for Delivery Center Plus** or contact sales – there’s no indication of separate pricing per domain[mxtoolbox.com](https://mxtoolbox.com/c/products/deliverycenterplus?source=FEAT-SPFF#:~:text=).

**Update Frequency & Monitoring:** MxToolbox doesn’t publish the technical details of how often it checks for changes, but as part of their managed service, we can infer it’s an **active/continuous monitoring** model. They highlight that their tool “keeps your email going” and “removes the limitations of SPF by reconstructing your SPF record automatically”[mxtoolbox.com](https://mxtoolbox.com/c/products/features/spf-flattening#:~:text=MxToolbox%27s%20SPF%20Flattening%20service%20gives,and%20keep%20your%20email%20going)[mxtoolbox.com](https://mxtoolbox.com/c/products/features/spf-flattening#:~:text=manage%20complex%20SPF%20configurations,reconstructing%20your%20SPF%20record%20automatically). In practice, when you add a third-party service or when one of your providers updates their ranges, MxToolbox’s system will update the hosted record. Given the premium nature, they likely ensure changes are picked up quickly (possibly monitoring known provider includes daily or better). The user gets a simplified SPF that **always stays under 10 lookups**, and MxToolbox handles all the background vendor checks. There is little public info on the interval (some services do 5 minutes; MxToolbox might do hourly or instant generation on-demand). Regardless, the model is **“centralized management”** – the user trusts MxToolbox to maintain accuracy of the SPF record over time.

**Integrations:** As a part of the Delivery Center Plus offering, the SPF flattening feature is integrated in the MxToolbox dashboard. Users of that plan can manage SPF alongside monitoring of DMARC, bounce reports, etc. There isn’t mention of an API specifically for updating SPF, since the expectation is you’ll use their UI to manage senders. Also, because it’s not offered separately, you can’t integrate this flattening service into your workflow without the rest of the MxToolbox platform. In short, it’s **bundled (not sold separately)** – you must subscribe to the full suite to get it. This integration model is suited for enterprises that want one vendor for all email deliverability tools, as opposed to a standalone SPF solution.

### **Valimail Instant SPF® (Dynamic SPF as Part of Valimail Enforce)**

**Offering & Delivery:** Valimail, a leading DMARC enforcement provider, takes a slightly different approach called **Instant SPF®**. Rather than static flattening, Valimail offers an **automated, on-the-fly SPF service** as part of its **Valimail Enforce** product[valimail.com](https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/#:~:text=The%20alternative%20to%20SPF%20flattening%3A,Valimail). The organization adds a special include or mechanism (Valimail uses a macro-based “exists” mechanism with a unique identifier) to their SPF record[reddit.com](https://www.reddit.com/r/sysadmin/comments/m1f17s/spf_flattening_recommendations/#:~:text=like%3A). This counts as only one lookup. When a receiving mail server checks SPF, Valimail’s cloud service **dynamically generates the appropriate response** containing all the authorized IPs _at query time_. In essence, Valimail’s DNS responds with a tailored SPF record in milliseconds for each request[valimail.com](https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/#:~:text=match%20at%20L357%20Valimail%20Instant,to%20each%20mail%20server%20request). This approach means you _never_ have to update your SPF manually nor worry about IP changes – the latest data is always served. Valimail markets this as solving the 10-lookup limit “without recourse to SPF flattening” (by which they mean without a static list of IPs that can go stale)[valimail.com](https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/#:~:text=The%20alternative%20to%20SPF%20flattening%3A,Valimail). It’s effectively a dynamic flattening service, leveraging Valimail’s cloud infrastructure. Setup involves delegating SPF resolution to Valimail (usually via an `include:spf.vali.email` or an `exists:` macro with a unique ID, as hinted in community discussions[reddit.com](https://www.reddit.com/r/sysadmin/comments/m1f17s/spf_flattening_recommendations/#:~:text=like%3A)).

**Pricing:** Valimail’s Instant SPF is **only available within the Valimail Enforce platform**, which is a high-end, enterprise-focused DMARC solution. Pricing is not publicly listed and is handled via sales (typically custom quotes). Given Valimail’s target market (large enterprises and government agencies), the cost is significantly higher than SMB-oriented services. It often scales by domain or email volume, and it bundles a host of features (DMARC policy management, dashboards, etc.) with Instant SPF as just one component[valimail.com](https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/#:~:text=match%20at%20L365%20Instant%20SPF,and%20stay%20at%20continuous%20enforcement). For context, Valimail does offer a free monitoring product (Valimail Monitor) but **Instant SPF is part of their paid enforcement tier**, likely costing in the thousands of dollars per year at minimum. They position it as an enterprise-grade solution – for instance, one might get DMARC enforcement plus unlimited dynamic SPF for, say, dozens of domains as part of a contract. There is no per-domain retail price known, and certainly no free tier for Instant SPF; it’s a premium offering bundled in an all-in-one service.

**Update Frequency & Monitoring:** Because Instant SPF is _dynamic_, there isn’t a periodic “polling” interval – instead, the system is continuously aware of all configured senders’ records. Valimail maintains a “global, cloud-based infrastructure” that knows the IP ranges of common senders (and likely checks them regularly)[valimail.com](https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/#:~:text=match%20at%20L357%20Valimail%20Instant,to%20each%20mail%20server%20request). Whenever an SPF check comes in, **Valimail generates the SPF data in real time**[valimail.com](https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/#:~:text=Valimail%20Instant%20SPF%20is%20the,to%20each%20mail%20server%20request). This means any change (even if a provider adds an IP this morning) is reflected immediately in responses. It offers the ultimate in currency of data – essentially zero lag. This is a key selling point for Valimail: that you get the benefit of flattening (no lookup excess, no maintenance) without the downside of stale records[valimail.com](https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/#:~:text=Automated%20solutions%20like%20Valimail%20Instant,advantages%20over%20manual%20SPF%20flattening). They often tout “no more SPF permerror, no more manual updates, set it and forget it” as advantages[valimail.com](https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/#:~:text=Automated%20solutions%20like%20Valimail%20Instant,advantages%20over%20manual%20SPF%20flattening)[valimail.com](https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/#:~:text=The%20alternative%20to%20SPF%20flattening%3A,Valimail). Indeed, Valimail’s literature argues that traditional SPF flattening “creates more problems” if done statically, and that an _automated alternative_ (their Instant SPF) is the correct solution[valimail.com](https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/#:~:text=SPF%20Flattening%3A%20What%20It%20Is,Why%20It%20Doesn%E2%80%99t%20Work)[valimail.com](https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/#:~:text=Automated%20solutions%20like%20Valimail%20Instant,advantages%20over%20manual%20SPF%20flattening).

**Integrations:** To use Instant SPF, a customer must onboard to Valimail Enforce, which involves configuring domains in their system and usually updating DNS records for SPF, DKIM, and DMARC to delegate control to Valimail. It’s fully integrated – once you’re a Valimail customer, you manage authorized senders via their interface. There you can add or remove email services (e.g., check a box for Salesforce, or add a custom mail server), and Valimail handles the rest. There’s no standalone API just for SPF (the entire service is API-driven under the hood, but as a customer you interface with their product as a whole). In summary, **Valimail’s dynamic SPF is bundled into a broader email authentication service**. It’s suitable for large organizations that want a comprehensive, hands-off solution and are willing to invest in a complete platform rather than a point solution.

### **Other Notable Mentions**

- **OnDMARC (Proofpoint)** – OnDMARC (from Red Sift, now part of Proofpoint) offers a feature similar to Instant SPF called **Dynamic SPF**. It gives you a record to include and then dynamically manages the SPF entries. The service flattens and **honors provider TTLs** for real-time updates[knowledge.ondmarc.redsift.com](https://knowledge.ondmarc.redsift.com/en/articles/3822217-how-does-dynamic-spf-work#:~:text=How%20does%20Dynamic%20SPF%20work%3F,includes%20honouring%20their%20published%20TTL). This is included in OnDMARC’s paid plans (which are tiered for SMB up to enterprise). OnDMARC’s pricing (before acquisition) ranged from a few hundred dollars per year for basic plans up to thousands, and Dynamic SPF was included in all levels. Essentially, it’s another integrated solution (no separate purchase) focusing on automation to keep SPF under the limit.
    
- **DMARC Advisor** – The Europe-based DMARC Advisor platform introduced an **automated SPF Flattening feature** as well. It flattens your SPF and _“monitors the SPF entries and automatically updates the SPF record when changes occur.”_[dmarcadvisor.com](https://dmarcadvisor.com/spf-management/#:~:text=SPF%20Management%3A%20Accurate%20and%20Up,SPF%20record%20when%20changes%20occur). This sounds very much like the others – one include that they keep current. DMARC Advisor markets it as _“the definitive end of Too Many DNS Lookups”_[dmarcadvisor.com](https://dmarcadvisor.com/spf-flattening/#:~:text=SPF%20Flattening%20by%20DMARC%20Advisor,maximum%20control%20with%20minimum%20risk). Again, this is part of their DMARC management service, available to their subscribers (mostly European clientele). Pricing for DMARC Advisor is similar to other DMARC suites; the SPF feature is bundled (they don’t sell it standalone).
    
- **Fraudmarc** – Fraudmarc is a smaller DMARC vendor that, according to community discussions, may offer an automated SPF service. It’s not entirely clear from their homepage, but they likely fall into the same category of bundling SPF management into a broader DMARC toolset[reddit.com](https://www.reddit.com/r/sysadmin/comments/m1f17s/spf_flattening_recommendations/#:~:text=records%20accordingly). Interested users would have to contact them to confirm how it works and what it costs, as details are sparse.
    
- **Dmarcian** – Notably, Dmarcian (one of the original DMARC analyzers) has historically advised against SPF flattening in its static form[dmarcian.com](https://dmarcian.com/spf-best-practices/#:~:text=SPF%20Best%20Practices%3A%20Avoiding%20SPF,your%20SPF%20records%20using). They provide an SPF checker and tools to _manually_ flatten an SPF record (and even had an “SPF Surveyor” tool), but they do **not** appear to offer a dynamic flattening service. In fact, a blog post from Dmarcian’s founder is titled “Avoiding SPF Record Flattening,” emphasizing the maintenance burden of static flattening[dmarcian.com](https://dmarcian.com/spf-best-practices/#:~:text=SPF%20Best%20Practices%3A%20Avoiding%20SPF,your%20SPF%20records%20using). As of 2025, Dmarcian’s service does not include an automated Safe SPF equivalent (their focus is more on monitoring DMARC/SPF compliance rather than hosting records). This stance further highlights that the dynamic SPF market has been led by newer companies and some established ones, while a few older players chose not to implement it.
    
- **Free Tools (DIY)** – There are free SPF flattening **generators** like those on DmarcDuty or provided by DMARCLY’s website[dmarcly.com](https://dmarcly.com/tools/spf-record-checker#:~:text=Free%20SPF%20Record%20Checker%20,button%2C%20then%20scroll%20down). These allow you to input an SPF record and get a flattened version (a static list of IPs) to copy into DNS. For example, **DmarcDuty** created a _Free SPF Flattening Tool_ that not only flattens your record but can periodically check if the flattened record is outdated[dmarcduty.com](https://dmarcduty.com/spf-flattening/step1#:~:text=Our%20free%20SPF%20flattening%20tool,). However, these free tools are not fully automated services – they don’t generally host a dynamic DNS include for you. Instead, they might send reminders or require you to come back and regenerate if something changes. They are useful for one-off fixes or very small domains (and as a teaser for paid solutions), but they lack the real-time monitoring and “single include” convenience of the paid services above. Technical users also sometimes **DIY their own flattening** by writing scripts or using open-source tools (for instance, periodically running a script that updates a DNS record via API)[reddit.com](https://www.reddit.com/r/sysadmin/comments/m1f17s/spf_flattening_recommendations/#:~:text=SPF%20Flattening%20Recommendations%20%3A%20r%2Fsysadmin,make%20your%20own%20flattening%20service). While feasible, that approach requires maintenance and doesn’t scale easily for non-experts, which is why the above commercial services exist.
    

Below is a **comparison table** summarizing the top SPF flattening services, their nature (standalone vs. suite), how they deliver the service, pricing, and whether the feature is bundled with other products or sold separately:

**Summary Comparison of SPF Flattening Services:**

|Service & Type|Delivery Method (Automation)|Pricing Structure|Bundled or Standalone?|Update Frequency & Notes|
|---|---|---|---|---|
|**AutoSPF**(Standalone)|Provides a unique **DNS include**(hosted on AutoSPF/Cloudflare) that flattens all your SPF includes to IPs. Fully automated – set up once and no further DNS changes needed.|~**$37/month** for 1 domain; $97/month for 5 domains; enterprise ~$387/month for 10 domains[autospf.com](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=,ideal%20for%20enterprise%20organizations%3A%20%24387%2Fmonth). No per-email limits. Free trial available (free use for one-click flattening)[reddit.com](https://www.reddit.com/r/sysadmin/comments/m1f17s/spf_flattening_recommendations/#:~:text=There%20are%20two%20types%20of,flattening%20service).|**Standalone service** – not part of a larger suite. Sole focus is SPF management[autospf.com](https://autospf.com/#:~:text=No%20Extra%20Suite%20of%20Tools,Needed).|**Every 5 minutes**active checks[autospf.com](https://autospf.com/#:~:text=We%20automate%20regular%20checks%20for,comes%20to%20your%20technology%20stack). Very fast updates to any vendor IP changes. 99.99% DNS SLA via Cloudflare[autospf.com](https://autospf.com/#:~:text=with%20an%20automatically%20managed%20SPF,availability%20is%20ensured%20by%20Cloudflare). Enterprise features (SSO, team roles) for MSPs.|
|**DMARCLY Safe SPF**(DMARC Suite)|One-time setup via **include record**provided by DMARCLY. The platform dynamically hosts a flattened SPF TXT for your domain(s). Managed through DMARCLY dashboard.|Included in DMARCLY plans (no separate cost). E.g., **$17.99/mo** plan includes 1 domain, $39.99/mo includes 2 domains, $69.99 includes ~3-4, $199/mo includes 4 Safe SPF domains[domainadmintools.com](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=you%20to%20manage%20unlimited%20domains,cheaper%20if%20you%20go%20yearly). Extra domains cost $10 each per month beyond plan quota[dmarcly.com](https://dmarcly.com/pricing#:~:text=,domains%20exceeds%20the%20plan%20quota).|**Bundled** with DMARC monitoring suite. Cannot purchase Safe SPF alone (comes with DMARCLY subscription)[domainadmintools.com](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=Dmarcly%20does%20not%20have%20any,Enterprise%20at%20%24199%20per%20month).|**Frequent automated updates** (occurs “every few minutes” in practice)[dmarcly.com](https://dmarcly.com/blog/what-is-automatic-dynamic-spf-record-flattening-and-how-it-fixes-the-spf-permerror-too-many-dns-lookups-issue#:~:text=Fortunately%2C%20SPF%20record%20flattening%20can,to%20the%20final%20SPF%20record). Monitors all included senders and auto-updates the record. Provided as part of a full DMARC solution (reports, BIMI, etc.).|
|**EasyDMARC EasySPF**(DMARC Suite)|**Dynamic include** via EasyDMARC’s cloud. Add a single include (e.g., `include:easyspf.easydmarc.com`) and manage SPF entries in the EasyDMARC portal. Changes in provider SPF are auto-applied[easydmarc.com](https://easydmarc.com/easy-spf-too-many-dns-lookups#:~:text=Add%20a%C2%A0single%20include%C2%A0in%20your%20SPF,and%20EasyDMARC%C2%A0takes%20care%C2%A0of%20the%20rest).|Included in plans. **Free tier** available (basic features; likely excludes dynamic SPF). Paid plans from **$17.99/mo** (Plus) for a couple domains, up to ~$39.99/mo (Premium) for 4+ domains[domainadmintools.com](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=Easydmarc%20has%20a%20FREE%20plan,cheaper%20if%20you%20go%20yearly)[domainadmintools.com](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=Price%20%28per%20month%29%2439,history%206%20months%201%20year). Annual discounts apply (~10% off)[domainadmintools.com](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=Price%20%28per%20month%29%2439,history%206%20months%201%20year).|**Bundled** with EasyDMARC suite. Not sold separately. Flattening is a feature called “EasySPF” in the platform.|**Continuously monitored** – EasySPF “detects and applies changes” to your SPF record automatically[easydmarc.com](https://easydmarc.com/tools/easy-spf#:~:text=management%20and%20results%20in%20an,immediate%20protocol%20failure). Essentially real-time updates (platform honors DNS changes from providers without user action). One include covers all your senders.|
|**PowerDMARC PowerSPF**(DMARC Suite)|**One-click flattening to single include** for multiple domains[autospf.com](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=PowerDMARC%20is%20a%20SaaS%20platform,changes%20made%20to%20authorized%20IPs). PowerDMARC hosts the SPF entries (compressed) and updates them dynamically. Managed in PowerDMARC dashboard (select your senders, etc.).|**Free plan** (1 domain, limited volume) available[autospf.com](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=,Enterprise%20Plan%3A%20Quote%20on%20Request). Paid plans start at **$8/month** (Basic) for more domains/emails[autospf.com](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=,Enterprise%20Plan%3A%20Quote%20on%20Request). Higher tiers by quote (Enterprise, MSP)[autospf.com](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=,MSP%2FMSSP%3A%20Quote%20on%20Request). Flattening is included in all tiers (even free).|**Bundled** with PowerDMARC SaaS. Not a standalone product, comes with the full email auth platform.|**Dynamic/Active updates** – stays **automatically up-to-date** with any changes in third-party IPs[autospf.com](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=PowerDMARC%20is%20a%20SaaS%20platform,changes%20made%20to%20authorized%20IPs). The platform ensures you “don’t exceed the 10 lookup limit” at any time[powerdmarc.com](https://powerdmarc.com/spf-rate-limit-permerror/#:~:text=A%20One,the%2010%20DNS%20lookup%20limit)[powerdmarc.com](https://powerdmarc.com/spf-rate-limit-permerror/#:~:text=stay%20updated%20on%20uninformed%20changes,the%2010%20DNS%20lookup%20limit). Frequency likely very high (near real-time).|
|**MxToolbox (Delivery Center Plus)**|**Hosted SPF record** via MxToolbox. Users get a flattened SPF managed by MxToolbox (likely through an include or direct TXT). All changes are done in their interface, and the DNS record is updated by them.|Only available in **Delivery Center Plus**plan at **$399/month**(includes 5 domains)[mxtoolbox.com](https://mxtoolbox.com/c/products/deliverycenterplus?source=FEAT-SPFF#:~:text=great%20features%20of%20Delivery%20Center%2C,for%20up%20to%205%20Domains). Not offered in cheaper plans. Effectively ~$80 per domain at that tier, along with many other features. No standalone pricing for just SPF.|**Bundled** with a broader deliverability suite (in highest tier). Not sold separately; must buy the full plan to get flattening[mxtoolbox.com](https://mxtoolbox.com/c/products/deliverycenterplus?source=FEAT-SPFF#:~:text=Mxtoolbox%20Delivery%20Center%20Plus%20has,for%20up%20to%205%20Domains).|**Automated updates** as part of managed service. Interval not explicitly stated, but expected to be proactive. The tool “reconstructs SPF automatically” to keep you under the limit[mxtoolbox.com](https://mxtoolbox.com/c/products/features/spf-flattening#:~:text=manage%20complex%20SPF%20configurations,reconstructing%20your%20SPF%20record%20automatically). A premium solution aimed at enterprise needs (with other email monitoring tools included).|
|**Valimail Instant SPF**(Enterprise DMARC)|**Macro-based dynamic SPF**delegation. Users publish a special include or `exists:` mechanism. Valimail’s cloud then answers SPF queries on-the-fly with current IPs[valimail.com](https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/#:~:text=Valimail%E2%80%99s%20solution%20is%20called%20Valimail,Valimail%20Enforce%20includes). No static record; it’s generated per email check.|Part of **Valimail Enforce** (enterprise subscription). **No public pricing**; sold via custom quotes. Typically part of a larger contract (likely $$$ per year). No standalone option or self-serve plan.|**Bundled** with Valimail’s DMARC enforcement service[valimail.com](https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/#:~:text=The%20alternative%20to%20SPF%20flattening%3A,Valimail). Not available standalone. Aimed at large organizations that need full-service DMARC + automation.|**Real-time dynamic** – generates a tailored SPF record **for each lookup**[valimail.com](https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/#:~:text=match%20at%20L357%20Valimail%20Instant,to%20each%20mail%20server%20request). Essentially instantaneous updates; if a sender’s IPs change, the next email’s SPF check already uses the new data. Completely removes maintenance overhead.|

_(Sources: Service websites and documentation as cited above. Pricing is current as of latest available data and may be subject to change. All services above provide continuously managed SPF flattening via DNS includes or similar mechanisms.)_

## **2. Proposed Pricing Model for SPFflatten.net**

Given the competitive landscape, we can now propose a pricing strategy for **SPFflatten.net** – a stand-alone continuously-managed SPF flattening service. The strategy will aim to balance competitiveness (to attract customers away from DIY or bundled solutions) with sustainability, considering the value provided (fully automated SPF maintenance). Key factors influencing pricing: competitors’ price points, the inclusion of free tiers in some offerings, and different needs of individuals vs. businesses vs. enterprises.

**Tiered Pricing Structure:** It would be wise for SPFflatten.net to offer **tiered monthly (and annual) plans**, segmented by number of domains (and possibly features) to serve different customer segments:

- **Free Tier (Individual/Basic)** – _“Personal” plan_: **Free**, for 1 domain (non-commercial use). This tier targets hobbyists, personal domains, or those wanting to test the service. Many competitors offer something at $0 (EasyDMARC’s free plan, PowerDMARC’s free, etc.)[domainadmintools.com](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=Easydmarc%20has%20a%20FREE%20plan,cheaper%20if%20you%20go%20yearly)[autospf.com](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=,Enterprise%20Plan%3A%20Quote%20on%20Request), so this matches market expectations. **Limits:** 1 domain, perhaps up to a modest email volume (if monitoring email volume) or limited support. The free tier could update less frequently (e.g. checks once per day) or not include certain advanced features. Its primary purpose is marketing: let users experience automated SPF flattening risk-free, and funnel serious users to paid plans. It also helps individuals who can’t justify a paid plan – given that EasyDMARC allows unlimited domains on free (but likely with very low email limits)[domainadmintools.com](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=Easydmarc%20has%20a%20FREE%20plan,cheaper%20if%20you%20go%20yearly), offering at least one free domain for flattening will make SPFflatten.net appealing in the community. We should mark the free tier as for **personal or trial use** (similar to PowerDMARC’s personal non-commercial free plan[autospf.com](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=,Enterprise%20Plan%3A%20Quote%20on%20Request)).
    
- **Basic Tier (SMB)** – _“Starter” or “Pro” plan_: **$X per month** for a limited number of domains (e.g. 3 domains). This plan is for small businesses or IT consultants managing a few domains. To be competitive, we suggest **pricing around $10–$15/month** for, say, up to 3 domains. For example, **$10/month for 3 domains** (if billed monthly; perhaps ~$100/year if billed annually to encourage annual sign-ups). This undercuts DMARCLY’s $17.99 plan which only includes 1 Safe SPF domain[domainadmintools.com](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=Dmarcly%20does%20not%20have%20any,Enterprise%20at%20%24199%20per%20month), and it significantly undercuts AutoSPF’s $37 for 1 domain[autospf.com](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=,ideal%20for%20enterprise%20organizations%3A%20%24387%2Fmonth) – positioning SPFflatten.net as a cost-effective alternative. Despite the lower price, this can be sustainable given the service is largely automated, and it will attract budget-conscious users who would otherwise try to flatten manually. **Limits:**Up to 3 domains (with maybe a reasonable limit on total DNS lookups per domain initially – e.g., can flatten up to, say, 50 includes per domain which is far above normal usage). The lookup-depth doesn’t need to be artificially limited (the service can handle recursive includes of any depth by design), so focus on domain count. The Basic plan would include core features: dynamic DNS include for each domain, checks at a decent frequency (perhaps every 15 or 30 minutes on this tier), email alert if changes occur, etc. No API access or team features on this plan (keep it simple for single users). Pricing at ~$10-15 is justified as an entry-level; it’s roughly **$3–$5 per domain** if 3 domains allowed, which is a good value compared to the ~$10 per domain that DMARCLY effectively charges for extras[dmarcly.com](https://dmarcly.com/pricing#:~:text=,domains%20exceeds%20the%20plan%20quota). It also aligns with the idea that flattening should be affordable as a standalone (since DMARC suites bundle it at higher cost mainly due to other features).
    
- **Professional Tier (SMB/Medium)** – _“Business” plan_: **$30–$50 per month** for a higher domain limit (e.g. 10 domains). This tier targets growing businesses or MSPs managing multiple client domains. For example, **$30/month for up to 10 domains** would price each domain at $3 – very competitive. Even at $50 for 10 domains, it’s $5 each, still below most competitors’ effective rates. AutoSPF’s equivalent (10 domains) is $387/mo[autospf.com](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=,ideal%20for%20enterprise%20organizations%3A%20%24387%2Fmonth), which is enterprise-grade pricing; SPFflatten.net at ~$30-50 for 10 domains would be a disruptive price point for mid-market customers. **Limits:** 10 domains included; perhaps allow more DNS traffic, and possibly faster update frequency than the basic tier. For instance, Business plan could check **every 5 minutes** (matching AutoSPF’s top speed)[autospf.com](https://autospf.com/#:~:text=We%20automate%20regular%20checks%20for,comes%20to%20your%20technology%20stack), while the Basic might check every 15 or 30. In practice, even 15-minute checks are usually fine, but offering the quickest updates at this tier adds value for those paying more. This plan might also include basic **API access or integrations** – e.g., an API to programmatically retrieve the flattened record or to push updates. That would attract MSPs who want to integrate SPFflatten into their workflow. Additionally, provide better support (e.g., priority email support) and perhaps **multiple user accounts** (so a small team can share access). The pricing again should be annually discounted (e.g., $300/yr if $30/mo, to incentivize yearly commitments). The key is to deliver far more domains per dollar than the big suites do, leveraging the singular focus of this service.
    
- **Enterprise Tier** – _“Enterprise” or “MSP” plan_: **Custom pricing or high-tier packages** for large-scale needs. For example, an Enterprise plan might be **$100/month for 25 domains**, $200/month for 50 domains, etc., with flexibility to extend. At this end, you might also offer **volume-based discounts** or custom quotes (similar to how DMARCLY and others go to “contact us” for large volumes). Enterprise tier should include **all advanced features**: highest check frequency (possibly on every DNS query like Valimail’s approach, though that might be technically different – but at least every 1 minute polls or instantaneous triggers if possible), **99.99% uptime SLA**, phone or 24/7 support, **integration APIs**, single sign-on (SAML) for large orgs, audit logs, and the ability to **white-label or CNAME** the include domain if needed. (For instance, an MSP might want `include:spf.yourMSP.com` that CNAMEs to SPFflatten’s records – enterprise could allow that kind of custom setup). Since enterprise users might compare the service to Valimail or to bundling with a DMARC provider, emphasize cost-effectiveness: e.g., **$200/month for 50 domains** is a fraction of typical enterprise DMARC solutions and could manage a whole portfolio of domains. Alternatively, a **Pay-as-you-go** model could be introduced here: e.g., $X per domain per month (maybe ~$5) if someone has a very large number of domains, with discounts beyond 100 domains, etc. However, keeping simple tiers (like 25, 50, 100 domain packs) might suffice. In line with DMARCLY’s overage of $10/domain[dmarcly.com](https://dmarcly.com/pricing#:~:text=,domains%20exceeds%20the%20plan%20quota), SPFflatten.net could even charge e.g. $5 per domain for enterprise volumes, undercutting that significantly while still generating revenue.
    

**Monthly vs Annual:** All prices above are monthly; offer an annual billing option with ~2 months free (i.e., ~16–17% discount, which is standard). For example, $10/mo becomes ~$100/yr if paid annually. This encourages commitment and upfront cash flow. It also aligns with competitors: DMARCLY and EasyDMARC show lower effective monthly prices with yearly plans[domainadmintools.com](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=Price%20%28per%20month%29%2439,history%206%20months%201%20year).

**Feature-based Differentiation:** Apart from domain count, consider differentiating tiers by features valuable to each segment:

- **Update Frequency:** As noted, perhaps **Daily** checks on Free (or manual re-check only), **hourly** on Basic, and **5-minute or real-time** on Business and Enterprise. However, one must be careful: even daily updates might be sufficient for many cases, but if a provider adds an IP and your SPF isn’t updated for 24 hours, there’s a window of potential SPF failure. To mitigate risk on lower tiers, SPFflatten.net could adopt at least twice-daily or hourly checks even for Basic. Another idea: The free tier might not actively monitor at all, but simply allow the user to click “re-flatten” in the UI to fetch updates (i.e., it’s a managed flattening tool but user-triggered on free). Paid = fully automatic. This encourages upgrading for peace of mind.
    
- **API Access & Integrations:** Offer API access starting at the **Business tier**. This would let users programmatically query the current flattened SPF record or integrate domain setup via API. For Enterprise, full API plus maybe integration assistance (scripts, etc.) can be included. Many SMBs won’t need an API, so Basic can omit it to keep things simple.
    
- **Support Level:** Free users get community or forum support (or knowledge base access), Basic gets email support (with 1-2 business day response), Business gets priority email (24-hour response), Enterprise gets phone/slack support with fast SLAs. This scaling is common and justifies higher prices to enterprises.
    
- **Lookup/Record Limits:** Ensure all plans handle a reasonable complexity of SPF. The service should allow _unlimited included senders per domain_ (within technical limits). However, there is a DNS TXT record size limit ~512 bytes; if a domain’s flattened SPF would exceed that, SPFflatten.net should automatically **split into multiple include records** (and instruct the user to include multiple entries) or use a macro-based approach. This is a technical solution that the service can implement transparently. There’s typically no need to put an artificial cap like “up to 50 IPs” – these scenarios are rare and can be handled by clever output (many services already handle long SPF by chunking). So, advertise **“no limit on number of senders/IPs – we’ll handle it”**. Only caveat: if a single domain uses an extreme number of providers (say 20+ includes), we might nudge them to Enterprise for performance reasons. But in pricing terms, that domain is just one domain. So it stays simple: limits by domain count, not by complexity. (If needed, an acceptable use clause can note that domains with extraordinary SPF size may require a custom plan, but this can be handled case by case.)
    
- **Target Segments:**
    
    - _Individuals_: free tier covers them.
        
    - _SMBs_: Basic and Business tiers at $10–$30 range are very affordable (especially when competitors require $18–$40 and often come with unused DMARC features).
        
    - _Enterprises_: Enterprise tier at, say, $100-$200+ gives a predictable cost for large portfolios, far less than enterprise DMARC services. For a company with, e.g., 20 domains, SPFflatten.net might be ~$100/month vs. a Valimail which could be tens of thousands per year. This value proposition can be highlighted.
        

**Competitive Justification:** The above pricing model leverages the fact that SPFflatten.net is **stand-alone** with a narrow focus, so it can be priced lower than full-suite offerings while still providing a high-quality solution. For instance, a small business that currently might buy a DMARC vendor plan at $18/mo mainly to get Safe SPF could instead get SPFflatten.net for ~$10 and use separate (perhaps free) DMARC reporting tools. Similarly, MSPs managing many client domains could find huge savings using SPFflatten.net’s enterprise packages instead of enrolling each client in a costly DMARC SaaS. By offering a free tier and a low-cost basic plan, SPFflatten.net mirrors the strategy of EasyDMARC and PowerDMARC that helped them gain market share (EasyDMARC’s free plan is a key differentiator[domainadmintools.com](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=Easydmarc%20has%20a%20FREE%20plan,cheaper%20if%20you%20go%20yearly), and PowerDMARC’s $8 basic lowered the entry barrier[autospf.com](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=,Enterprise%20Plan%3A%20Quote%20on%20Request)). We’ve undercut even those prices slightly, which should attract users – as long as the service is reliable and trustworthy (which it should be, with strong uptime and security measures).

**Monthly vs. Tiered Domain Add-Ons:** Another angle – we could offer simple **per-domain pricing** (e.g. $5/domain/month) with volume discounts. However, customers generally prefer predictable tiers, and per-unit pricing can become confusing or expensive at low end (e.g., $5 for one domain is okay, but we’d want $10 min perhaps). Tiered bundles as proposed are easier to market (“flatten up to X domains for $Y”). We can also allow adding extra domains à la carte for a small fee on lower plans (similar to DMARCLY’s $10/additional) – for example, Basic plan includes 3 domains, additional domains $5 each; Business includes 10, additional $3 each, etc. This flexibility ensures users don’t have to jump fully to the next tier for just one more domain.

**Annual Plans for Stability:** Encourage annual subscriptions by pricing them attractively (e.g., 2 months free). This also aligns with business budgeting cycles. For instance, Business at $30/mo is $300/yr (instead of $360), which sounds even more appealing when marketing (“just $25/month equivalent on annual plan”).

In summary, **our proposed pricing tiers for SPFflatten.net** might look like:

- _Free_: 1 domain, manual or daily updates, community support. – **$0**.
    
- _Basic_: 3 domains, automated (hourly) updates, email support. – **$10/month** ( ~$100/year).
    
- _Business_: 10 domains, fast updates (5-min), API access, priority support. – **$30/month** (~$300/year).
    
- _Enterprise_: 25+ domains, fastest updates (real-time), API, SLA, SSO, premium support. – **$100/month for 25** (scaling upward; or contact for custom quote if many more domains).
    

These numbers can be adjusted (e.g., $15 and $50 instead of $10 and $30) based on further market testing, but the key idea is to stay **well below the cost of alternative solutions for comparable domain counts**, highlighting SPFflatten.net as both **affordable** and specialized (you pay only for SPF flattening, not for extras you may not use).

By basing the limits on _number of domains_, we directly tie to how competitors structure it (DMARCLY Safe SPF count, AutoSPF charges by domains). We consciously do **not impose an email volume limit** on SPFflatten.net plans (following AutoSPF’s approach of unlimited emails[autospf.com](https://autospf.com/#:~:text=No%20email%20sending%20limits%2C%20no,based%20pricing)). Email volume mainly affects DMARC processing (because of report sizes), but for SPF flattening, the workload is on DNS queries and monitoring includes – which is more related to number of domains and providers than the number of emails sent. So unlimited sending is fine – one less worry for customers (AutoSPF also advertises “no email sending limits”[autospf.com](https://autospf.com/#:~:text=No%20email%20sending%20limits%2C%20no,based%20pricing)).

Overall, this pricing model should allow SPFflatten.net to gain traction among tech-savvy users and small businesses (through the free/basic options) and then monetize growth (with the business/enterprise tiers as they add more domains or require more assurances).

## **3. Integration Strategy with mxio.io**

**mxio.io** is a broader suite of email administration tools, and SPFflatten.net is (or will be) one service under the same company’s umbrella. We need to recommend how to position the SPF flattening service relative to the mxio.io suite. The options are: keep it separate (with cross-promotion), fully integrate it as a core feature of mxio.io’s paid plans, or offer it as an optional paid add-on for mxio.io users (possibly with bundle discounts). Each approach has pros and cons, and the optimal strategy may even combine elements of these models:

- **Option 1: Keep SPFflatten.net as a Separate Service but Cross-Promote**  
    In this model, **mxio.io and SPFflatten.net remain distinct products/brands**, each available on their own. Users can sign up for SPF flattening without being mxio.io customers, and vice versa. However, the two would be marketed in tandem where appropriate. For example, mxio.io’s website can mention “Need to solve SPF lookup limits? Try our sister service SPFflatten.net” and SPFflatten.net’s site can have banners or menu links highlighting “SPF flattening is part of the mxio.io email toolbox – check out our other tools.” The advantage here is capturing a wider audience: some customers will search specifically for “SPF flattening service” and not want a full suite – the standalone site caters to them (much like AutoSPF targets those who just want SPF fixed)[autospf.com](https://autospf.com/#:~:text=No%20Extra%20Suite%20of%20Tools,Needed). Meanwhile, mxio.io can focus on its broader feature set, and not everyone using mxio.io may need flattening if they don’t hit the SPF limit. By keeping it separate, you also preserve pricing flexibility; you can optimize SPFflatten.net’s pricing for flattening, and mxio.io’s pricing for its suite, without entangling the two.
    
    **Pros:**  
    – **Focused Marketing:** SPFflatten.net can become an authority in its niche, attracting users via SEO (people specifically searching for SPF solutions) without confusing them with unrelated features.  
    – **Conversion Funnel:** It can serve as a lead generator – e.g., a small business owner finds SPFflatten.net to fix an issue, and once on board, they learn about mxio.io’s other offerings (like DMARC monitoring, SMTP tools, etc.), potentially upselling them to the larger suite.  
    – **Development Independence:** The SPF service can evolve rapidly on its own, and mxio.io’s suite can use it via APIs or backend integration, without needing to sync release cycles or architectures tightly. Essentially, mxio.io could “use” SPFflatten.net under the hood for its customers, even if it’s a separate service, by calling its API. This separation of concerns can speed up improvements.  
    – **Appeals to Non-suite Customers:** Some clients might never buy a whole suite (maybe they already have other tools), but they would buy a point solution. This model lets you monetize those customers rather than losing them if flattening was only inside mxio.io.
    
    **Cons:**  
    – **Dual Branding Overhead:** You have to maintain two brands, websites, possibly two billing systems (though you can unify on the backend). Marketing efforts are split.  
    – **Potential Confusion:** Customers might not immediately realize both services come from the same company. It will require clear communication to ensure trust (“mxio.io’s team also brings you SPFflatten.net”) so they know it’s a cohesive ecosystem, not a random third-party.  
    – **Integration Complexity for Users:** If a user wants both, do they manage them separately? Ideally, we’d allow single sign-on between mxio.io and SPFflatten.net to smooth this. But if kept entirely separate, a user might have two accounts and dashboards. Cross-integration (like showing SPF data in mxio’s interface) would need to use APIs.
    
- **Option 2: Include SPF Flattening as a Core Feature in mxio.io Plans**  
    This approach would fold the SPF flattening functionality directly into the mxio.io platform. Essentially, mxio.io’s paid plans (or at least certain tiers) would have “Managed SPF Flattening” as a built-in feature – no separate sign-up or site; a mxio.io user would simply have the option to set up an automated SPF include for their domains from within the mxio interface. This is what many DMARC suites do: for instance, DMARCLY and EasyDMARC include SPF flattening in even their base plans[domainadmintools.com](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=Dmarcly%20does%20not%20have%20any,Enterprise%20at%20%24199%20per%20month)[domainadmintools.com](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=Price%20%28per%20month%29%2439,history%206%20months%201%20year), and it’s seen as part of the whole email compliance package. If mxio.io aims to compete with those suites, having flattening _included_ could make the offer more compelling. A mxio.io customer would get an “all-in-one” solution: not only monitoring or tools but also the fix for SPF issues. Including it could justify a higher price for mxio.io’s plans or simply be a value-add differentiator.
    
    **Pros:**  
    – **One-Stop Shop Value:** It increases the value proposition of mxio.io. Customers paying for an email admin suite likely expect it to handle major pain points; SPF is one of them. By bundling flattening, you meet that expectation. This could tip the scales when customers compare mxio.io to another toolset that might not include flattening.  
    – **Simplicity for Users:** A single login and platform to manage everything. They don’t need to juggle separate services. In mxio.io’s dashboard, they could see DMARC reports, adjust DNS records, and also configure SPF flattening. This unified experience is convenient.  
    – **Competitive Alignment:** As noted, key competitors in the DMARC realm include flattening as a core feature, not an extra cost (except at the very high end like MxToolbox). If mxio.io targets similar customer segments, including flattening ensures you’re not at a feature disadvantage. For example, an MSP comparing solutions might see that with mxio.io they have to also buy SPFflatten or do SPF manually, whereas with EasyDMARC it’s built-in – that could sway them. If included, mxio.io is on par or better.  
    – **No Fragmentation in Branding:** Everything under mxio.io name might build more brand strength. Instead of promoting two names, you invest in one brand, and that brand stands for a comprehensive suite.
    
    **Cons:**  
    – **Missed Revenue from Standalone Market:** By only offering it inside mxio.io, you might lose those users who only want SPF flattening and nothing else. Some smaller clients or individuals might shy away from a full suite subscription and thus not come at all. (However, you could mitigate this by having a very low-tier mxio.io plan basically just for SPF, but then that’s effectively making a separate offering anyway.)  
    – **Increased Load on mxio.io Support/Infrastructure:** Supporting flattening for potentially all customers means more infrastructure cost (DNS queries, monitoring jobs) that needs to be factored into mxio.io’s pricing. If not done carefully, some heavy SPF users could drive up costs while paying just the normal plan fee. We’d need to bake in those costs or set reasonable usage policies.  
    – **All-or-Nothing for Users:** If flattening is only available via mxio.io, a customer who has an existing toolset for other things but just needs SPF help might not want to switch everything to mxio.io. We’d force them into a bigger commitment, possibly turning them away. Some customers prefer best-of-breed point solutions rather than an all-in-one if they already have partial solutions.  
    – **Development Complexity:** Integrating flattening deeply means the mxio.io platform will need to incorporate those workflows (domain include provisioning, etc.) and perhaps data from the flattening system. It’s doable (especially if internally it just calls SPFflatten’s engine), but it ties the release cycles together.
    
- **Option 3: Offer SPF Flattening as a Paid Add-On to mxio.io**  
    This is a middle-ground approach. mxio.io users could choose to **add SPF flattening to their account for an extra fee** (possibly at a discounted rate versus buying the standalone service). For instance, mxio.io could have a “Premium Add-on: Automated SPF Management for X domains – $Y/month” that can be attached to any plan. This is somewhat like how some services operate: e.g., a base product with optional add-ons like enhanced support, extra features, etc. MxToolbox’s approach of having “Plus” vs standard is akin to an add-on model (except they bundle multiple add-ons in Plus)[mxtoolbox.com](https://mxtoolbox.com/c/products/deliverycenterplus?source=FEAT-SPFF#:~:text=Mxtoolbox%20Delivery%20Center%20Plus%20has,for%20up%20to%205%20Domains). We could allow à la carte enabling of SPF flattening per domain or in packs. The UI would then reveal SPF flattening settings when the add-on is active.
    
    **Pros:**  
    – **Monetization of Feature:** You don’t give it away entirely free with the suite, thus you can generate additional revenue specifically from this high-value feature. For example, mxio.io could advertise plans and then “Add SPF Auto-Flattening for 5 domains – +$15/month.” This might increase ARPU (average revenue per user) for those who opt in, while keeping base plan prices lower for those who don’t need it.  
    – **Flexibility for Users:** Customers only pay for flattening if they need it. If a user’s domain never approaches the lookup limit, they might skip the add-on. Those who are hitting the limit can enable it. This way, mxio.io’s base price can remain competitive and not “subsidize” the flattening cost for everyone. Essentially, it’s pay-as-you-go for that functionality.  
    – **Cross-Promotion Synergy:** If the user is already in mxio.io, seeing an add-on might be easier to upsell (“click here to activate automatic SPF management”). Conversely, someone coming through SPFflatten.net’s door could be upsold: “Did you know mxio.io can also manage your DMARC, etc? If you have an mxio.io account, you get a discount on this service.” Offering a **bundle discount** would sweeten the add-on model – e.g., if you subscribe to both mxio.io and SPF flattening, perhaps there’s, say, a 20% discount on the combined price. This encourages existing customers of one to become customers of the other, increasing lifetime value.  
    – **Maintains Separate Identity & Quality:** SPFflatten.net can continue to exist as a standalone (for external customers), but mxio.io users don’t have to leave the mxio interface to use it – it appears as a feature once purchased. Internally, the systems could remain somewhat separate (with an integration layer), allowing specialized development, but to the user it feels integrated. This gives a best-of-both-worlds in many ways.
    
    **Cons:**  
    – **User Experience Complexity:** Add-ons can confuse some customers or add friction. People might wonder, “Should I get this? Why isn’t it just included?” If not communicated well, one might feel nickel-and-dimed (“mxio.io charges extra for SPF management whereas competitor X includes it”). It will be important to position it positively (e.g., base plans for those who don't need it, and a fair price for those who do).  
    – **Pricing Balancing:** We’d have to be careful in pricing the add-on relative to standalone and relative to plan prices. If priced too high, users might feel it should’ve been included. If too low, we might undercut our own standalone service. A balance might be, for instance, an mxio.io user can add flattening at, say, 20% off the standalone price they’d pay if they went directly. This rewards loyalty to the ecosystem.  
    – **Integration Effort:** This still requires building integration points in mxio.io (though not as deeply as option 2). The system needs to know if a user has the add-on, and either embed frames from SPFflatten.net or replicate certain UI components. Ensuring a seamless experience (maybe SSO between platforms, unified billing) is a bit of extra work but certainly feasible.
    

**Recommended Approach:** After considering the above, a **hybrid strategy leaning towards Option 3 (Add-On model)** appears most advantageous. Here’s how I would implement it:

- **Keep SPFflatten.net operating as an independent service and brand** to capture outside customers and establish reputation in that niche. Continue to develop it aggressively as a top-notch tool. This covers the standalone market and serves as a lead funnel.
    
- **Integrate SPFflatten into mxio.io as an optional add-on**: On the mxio.io dashboard, paying users can enable “SPF Flattening” for their domains with one click (after agreeing to an add-on charge). Technically, this could tie into the same backend as SPFflatten.net. Commercially, we can price this add-on at roughly the same levels discussed for SPFflatten.net (maybe slightly discounted). For example, an mxio.io Pro Plan user might pay +$10/month to flatten up to 3 domains. Because they’re already a customer, we could even allow a **free trial of the add-on for a month** to demonstrate its value, then charge thereafter.
    
- **Bundle in higher-tier mxio.io plans**: We can incorporate flattening **for free in the top-tier mxio.io plans**as a bonus. This is similar to how MxToolbox includes flattening only in their highest tier[mxtoolbox.com](https://mxtoolbox.com/c/products/deliverycenterplus?source=FEAT-SPFF#:~:text=great%20features%20of%20Delivery%20Center%2C,for%20up%20to%205%20Domains). For instance, if mxio.io has a tier for Enterprises, we could state “includes automated SPF flattening for X domains at no extra cost.” This encourages upgrades to that tier. Meanwhile, mid-tier or basic mxio plans can still get it via add-on if needed. This way, heavy users either way are paying more (either via tier or add-on), which covers the resource usage.
    
- **Cross-Promotion Mechanism:** Ensure both websites mention the other:
    
    - On **SPFflatten.net**: In the footer or as a section, mention “SPFflatten.net is part of the mxio.io family of email tools – if you need comprehensive email admin tools (DMARC reports, deliverability analytics, etc.), check out **mxio.io**.” This can drive standalone customers to explore the suite if they have broader needs.
        
    - On **mxio.io**: in the features page or knowledge base, mention “Our SPF Flattening service is powered by SPFflatten.net – a dedicated SPF optimization service we offer.” If someone googles and finds mxio.io, they become aware we have this solution (even if they didn’t initially think of it). Possibly even have a dedicated tab in mxio’s interface for “SPF Flattening” that explains the benefits and prompts enabling the feature (for an extra fee if not included).
        

**Why this hybrid approach?** It effectively captures all segments:

- Individuals/small users who only want SPF help can go directly to SPFflatten.net (no need to commit to a suite).
    
- Organizations already invested in mxio.io can conveniently add flattening without going elsewhere, strengthening their loyalty to the platform.
    
- We still maintain a clear identity for the flattening service, which fosters trust (some customers might feel a specialized service does it better – AutoSPF uses that argument of 100% focus[autospf.com](https://autospf.com/#:~:text=No%20Extra%20Suite%20of%20Tools,Needed); we can say the same for SPFflatten.net even if it’s under the same company).
    
- Revenue streams: standalone sales + add-on sales. The sum likely exceeds what we’d get by just bundling it into a single product price.
    

**Potential Pitfall and Mitigation:** One potential downside is **perception** – if mxio.io markets itself as an all-in-one but then charges extra for flattening, some prospects might question that. To mitigate this:

- Keep the base pricing of mxio.io competitive (so even plus the add-on it’s on par or cheaper than a competitor’s combined offering).
    
- Clearly articulate that flattening is a resource-intensive premium feature and not every domain needs it; thus, pricing it separately keeps costs fair for those who _do_ need it without raising prices for those who don't. This transparency can be appreciated by cost-conscious customers.
    
- Possibly include at least basic flattening in all paid plans as a teaser (for example, each mxio.io plan might include 1 domain flattening free, since DMARCLY’s lowest includes 1[domainadmintools.com](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=Dmarcly%20does%20not%20have%20any,Enterprise%20at%20%24199%20per%20month)). Then charge add-on for additional domains. This way every paying customer gets a taste of it as part of the value. For instance, mxio.io base plan: includes flattening for 1 domain; if they have more domains or want more, they buy add-on. This hybrid of bundling+addon could be very well-received.
    

**Incorporation with Minimal Friction:** Technically, mxio.io could integrate by using the same backend of SPFflatten.net via API. The user might not even need to know SPFflatten.net exists in that scenario – they click “enable”, the system calls the SPFflatten service to create the record and returns the include for the user to add to DNS (or if mxio provides DNS hosting, possibly auto adds it). Meanwhile, separate SPFflatten.net customers use the SPFflatten portal directly. Over time, if mxio.io’s user base overlaps greatly with flattening, the company could decide to more tightly merge them – but initially, keeping a degree of separation as recommended allows flexibility.

**Long-term Recommendation:** Start with the **add-on model**. Monitor adoption:

- If a large portion of mxio.io users take the add-on, and it becomes a hallmark feature, you might eventually fold it into core plans (because it’s clearly expected).
    
- If few add it, perhaps bundling it (to drive more usage) or promoting it more heavily might be needed.
    
- Maintain the standalone route regardless, because it’s capturing a different slice of the market (some of whom may convert to full mxio.io customers later).
    

In conclusion, **SPFflatten.net should be incorporated as an optional but well-integrated component of mxio.io**. The services should complement each other:

- **Cross-promote** to maximize visibility.
    
- **Bundle for high-end plans** to increase mxio.io’s attractiveness.
    
- **Offer as add-on for others** to generate incremental revenue and flexibility.
    
- All the while, **retain a standalone presence** to not miss out on the segment that just needs this one solution.
    

This approach ensures that the SPF flattening service both stands on its own merits and boosts the overall mxio.io ecosystem – driving growth for the company on multiple fronts.

Citations

[

![Favicon](https://www.google.com/s2/favicons?domain=https://dmarcreport.com&sz=32)

Flatten SPF Files to Ensure Email Deliverability – DMARC Report

https://dmarcreport.com/flatten-spf-files-to-ensure-email-deliverability/

](https://dmarcreport.com/flatten-spf-files-to-ensure-email-deliverability/#:~:text=AutoSPF%20automatically%20flattens%20SPF%20records%2C,process%20is%20highly%20automated%2C%20inherently)[

![Favicon](https://www.google.com/s2/favicons?domain=https://autospf.com&sz=32)

- AutoSPF - Automatic SPF flattening

https://autospf.com/

](https://autospf.com/#:~:text=We%20automate%20regular%20checks%20for,comes%20to%20your%20technology%20stack)[

![Favicon](https://www.google.com/s2/favicons?domain=https://autospf.com&sz=32)

- AutoSPF - Automatic SPF flattening

https://autospf.com/

](https://autospf.com/#:~:text=100)[

![Favicon](https://www.google.com/s2/favicons?domain=https://autospf.com&sz=32)

- AutoSPF - Automatic SPF flattening

https://autospf.com/

](https://autospf.com/#:~:text=,of%20additional%20networks%20and%20senders)[

![Favicon](https://www.google.com/s2/favicons?domain=https://autospf.com&sz=32)

- AutoSPF - Automatic SPF flattening

https://autospf.com/

](https://autospf.com/#:~:text=with%20an%20automatically%20managed%20SPF,availability%20is%20ensured%20by%20Cloudflare)[

![Favicon](https://www.google.com/s2/favicons?domain=https://autospf.com&sz=32)

- AutoSPF - Automatic SPF flattening

https://autospf.com/

](https://autospf.com/#:~:text=We%20automate%20regular%20checks%20for,comes%20to%20your%20technology%20stack)[

![Favicon](https://www.google.com/s2/favicons?domain=https://autospf.com&sz=32)

A Guide to Choosing the Right SPF Flattening Tool for Your Organization - AutoSPF - Automatic SPF flattening

https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/

](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=,ideal%20for%20enterprise%20organizations%3A%20%24387%2Fmonth)[

![Favicon](https://www.google.com/s2/favicons?domain=https://autospf.com&sz=32)

- AutoSPF - Automatic SPF flattening

https://autospf.com/

](https://autospf.com/#:~:text=No%20email%20sending%20limits%2C%20no,based%20pricing)[

![Favicon](https://www.google.com/s2/favicons?domain=https://www.reddit.com&sz=32)

SPF Flattening Recommendations : r/sysadmin

https://www.reddit.com/r/sysadmin/comments/m1f17s/spf_flattening_recommendations/

](https://www.reddit.com/r/sysadmin/comments/m1f17s/spf_flattening_recommendations/#:~:text=There%20are%20two%20types%20of,flattening%20service)[

![Favicon](https://www.google.com/s2/favicons?domain=https://autospf.com&sz=32)

- AutoSPF - Automatic SPF flattening

https://autospf.com/

](https://autospf.com/#:~:text=availability%20is%20ensured%20by%20Cloudflare)[

![Favicon](https://www.google.com/s2/favicons?domain=https://autospf.com&sz=32)

- AutoSPF - Automatic SPF flattening

https://autospf.com/

](https://autospf.com/#:~:text=%2A%20White,free%20%E2%80%93%20on%20us%21)[

![Favicon](https://www.google.com/s2/favicons?domain=https://autospf.com&sz=32)

- AutoSPF - Automatic SPF flattening

https://autospf.com/

](https://autospf.com/#:~:text=Set%20it%20up%20once%2C%20without,the%20need%20for%20regular%20logins)[

![Favicon](https://www.google.com/s2/favicons?domain=https://autospf.com&sz=32)

- AutoSPF - Automatic SPF flattening

https://autospf.com/

](https://autospf.com/#:~:text=,Vendor%20Security%20Questionnare)[

![Favicon](https://www.google.com/s2/favicons?domain=https://autospf.com&sz=32)

- AutoSPF - Automatic SPF flattening

https://autospf.com/

](https://autospf.com/#:~:text=No%20Extra%20Suite%20of%20Tools,Needed)[

![Favicon](https://www.google.com/s2/favicons?domain=https://dmarcly.com&sz=32)

What Is Automatic/Dynamic SPF Record Flattening and How It Fixes the SPF PermError: Too Many DNS Lookups Issue - DMARCLY

https://dmarcly.com/blog/what-is-automatic-dynamic-spf-record-flattening-and-how-it-fixes-the-spf-permerror-too-many-dns-lookups-issue

](https://dmarcly.com/blog/what-is-automatic-dynamic-spf-record-flattening-and-how-it-fixes-the-spf-permerror-too-many-dns-lookups-issue#:~:text=DMARCLY%27s%20Safe%20SPF%20feature%20implements,the%20DNS%20settings%20manually%20anymore)[

![Favicon](https://www.google.com/s2/favicons?domain=https://dmarcly.com&sz=32)

What Is Automatic/Dynamic SPF Record Flattening and How It Fixes the SPF PermError: Too Many DNS Lookups Issue - DMARCLY

https://dmarcly.com/blog/what-is-automatic-dynamic-spf-record-flattening-and-how-it-fixes-the-spf-permerror-too-many-dns-lookups-issue

](https://dmarcly.com/blog/what-is-automatic-dynamic-spf-record-flattening-and-how-it-fixes-the-spf-permerror-too-many-dns-lookups-issue#:~:text=final%20SPF%20record)[

![Favicon](https://www.google.com/s2/favicons?domain=https://dmarcly.com&sz=32)

What Is Automatic/Dynamic SPF Record Flattening and How It Fixes the SPF PermError: Too Many DNS Lookups Issue - DMARCLY

https://dmarcly.com/blog/what-is-automatic-dynamic-spf-record-flattening-and-how-it-fixes-the-spf-permerror-too-many-dns-lookups-issue

](https://dmarcly.com/blog/what-is-automatic-dynamic-spf-record-flattening-and-how-it-fixes-the-spf-permerror-too-many-dns-lookups-issue#:~:text=Fortunately%2C%20SPF%20record%20flattening%20can,to%20the%20final%20SPF%20record)[

![Favicon](https://www.google.com/s2/favicons?domain=https://domainadmintools.com&sz=32)

Easydmarc vs Dmarcly - Domain Admin Tools

https://domainadmintools.com/easydmarc-vs-dmarcly/

](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=you%20to%20manage%20unlimited%20domains,cheaper%20if%20you%20go%20yearly)[

![Favicon](https://www.google.com/s2/favicons?domain=https://dmarcly.com&sz=32)

DMARCLY Pricing

https://dmarcly.com/pricing

](https://dmarcly.com/pricing#:~:text=,DMARC%20compliant%20messages%20per%20month)[

![Favicon](https://www.google.com/s2/favicons?domain=https://dmarcly.com&sz=32)

DMARCLY Pricing

https://dmarcly.com/pricing

](https://dmarcly.com/pricing#:~:text=,domains%20exceeds%20the%20plan%20quota)[

![Favicon](https://www.google.com/s2/favicons?domain=https://domainadmintools.com&sz=32)

Easydmarc vs Dmarcly - Domain Admin Tools

https://domainadmintools.com/easydmarc-vs-dmarcly/

](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=match%20at%20L41%20Dmarcly%20does,Enterprise%20at%20%24199%20per%20month)[

![Favicon](https://www.google.com/s2/favicons?domain=https://dmarcly.com&sz=32)

What Is Automatic/Dynamic SPF Record Flattening and How It Fixes the SPF PermError: Too Many DNS Lookups Issue - DMARCLY

https://dmarcly.com/blog/what-is-automatic-dynamic-spf-record-flattening-and-how-it-fixes-the-spf-permerror-too-many-dns-lookups-issue

](https://dmarcly.com/blog/what-is-automatic-dynamic-spf-record-flattening-and-how-it-fixes-the-spf-permerror-too-many-dns-lookups-issue#:~:text=Fortunately%2C%20SPF%20record%20flattening%20can,to%20the%20final%20SPF%20record)[

![Favicon](https://www.google.com/s2/favicons?domain=https://easydmarc.com&sz=32)

Overcome SPF record permerror, automate email authentication

https://easydmarc.com/easy-spf-too-many-dns-lookups

](https://easydmarc.com/easy-spf-too-many-dns-lookups#:~:text=Add%20a%C2%A0single%20include%C2%A0in%20your%20SPF,and%20EasyDMARC%C2%A0takes%20care%C2%A0of%20the%20rest)[

![Favicon](https://www.google.com/s2/favicons?domain=https://easydmarc.com&sz=32)

EasySPF | Automatic SPF Configuration | EasyDMARC

https://easydmarc.com/tools/easy-spf

](https://easydmarc.com/tools/easy-spf#:~:text=management%20and%20results%20in%20an,immediate%20protocol%20failure)[

![Favicon](https://www.google.com/s2/favicons?domain=https://easydmarc.com&sz=32)

EasySPF | Automatic SPF Configuration | EasyDMARC

https://easydmarc.com/tools/easy-spf

](https://easydmarc.com/tools/easy-spf#:~:text=EasySPF%20eliminates%20the%20constant%20back,automatically%20depicted%20in%20the%20DNS)[

![Favicon](https://www.google.com/s2/favicons?domain=https://easydmarc.com&sz=32)

EasySPF | Automatic SPF Configuration | EasyDMARC

https://easydmarc.com/tools/easy-spf

](https://easydmarc.com/tools/easy-spf#:~:text=EasySPF%20uses%20a%20dynamic%20SPF,record%20is%20up%20to%20date)[

![Favicon](https://www.google.com/s2/favicons?domain=https://easydmarc.com&sz=32)

Overcome SPF record permerror, automate email authentication

https://easydmarc.com/easy-spf-too-many-dns-lookups

](https://easydmarc.com/easy-spf-too-many-dns-lookups#:~:text=its%20own%20email%20authentication%20configurations,action%20to%20solve%20the%20issue)[

![Favicon](https://www.google.com/s2/favicons?domain=https://easydmarc.com&sz=32)

Overcome SPF record permerror, automate email authentication

https://easydmarc.com/easy-spf-too-many-dns-lookups

](https://easydmarc.com/easy-spf-too-many-dns-lookups#:~:text=,changed%20over%20time)[

![Favicon](https://www.google.com/s2/favicons?domain=https://domainadmintools.com&sz=32)

Easydmarc vs Dmarcly - Domain Admin Tools

https://domainadmintools.com/easydmarc-vs-dmarcly/

](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=Easydmarc%20has%20a%20FREE%20plan,cheaper%20if%20you%20go%20yearly)[

![Favicon](https://www.google.com/s2/favicons?domain=https://domainadmintools.com&sz=32)

Easydmarc vs Dmarcly - Domain Admin Tools

https://domainadmintools.com/easydmarc-vs-dmarcly/

](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=Price%20%28per%20month%29%2439,history%206%20months%201%20year)[

![Favicon](https://www.google.com/s2/favicons?domain=https://support.openprovider.eu&sz=32)

EasyDMARC Frequently Asked Questions – Openprovider

https://support.openprovider.eu/hc/en-us/articles/5294664442386--EasyDMARC-Frequently-Asked-Questions

](https://support.openprovider.eu/hc/en-us/articles/5294664442386--EasyDMARC-Frequently-Asked-Questions#:~:text=We%20strongly%20recommend%20using%20CNAME,DNS%20zone%20settings%20every%20time)[

![Favicon](https://www.google.com/s2/favicons?domain=https://powerdmarc.com&sz=32)

Automatic SPF Flattening Tool: Remove Extra Lookups

https://powerdmarc.com/spf-rate-limit-permerror/

](https://powerdmarc.com/spf-rate-limit-permerror/#:~:text=Your%20Dynamic%20and%20Hassle,Flattening%20tool)[

![Favicon](https://www.google.com/s2/favicons?domain=https://powerdmarc.com&sz=32)

Automatic SPF Flattening Tool: Remove Extra Lookups

https://powerdmarc.com/spf-rate-limit-permerror/

](https://powerdmarc.com/spf-rate-limit-permerror/#:~:text=How%20Our%20SPF%20Flattening%20Tool,Works)[

![Favicon](https://www.google.com/s2/favicons?domain=https://autospf.com&sz=32)

A Guide to Choosing the Right SPF Flattening Tool for Your Organization - AutoSPF - Automatic SPF flattening

https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/

](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=PowerDMARC%20is%20a%20SaaS%20platform,changes%20made%20to%20authorized%20IPs)[

![Favicon](https://www.google.com/s2/favicons?domain=https://autospf.com&sz=32)

A Guide to Choosing the Right SPF Flattening Tool for Your Organization - AutoSPF - Automatic SPF flattening

https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/

](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=,Enterprise%20Plan%3A%20Quote%20on%20Request)[

![Favicon](https://www.google.com/s2/favicons?domain=https://www.softwaresuggest.com&sz=32)

PowerDMARC Pricing, Features, and Reviews (Jun 2025)

https://www.softwaresuggest.com/powerdmarc

](https://www.softwaresuggest.com/powerdmarc#:~:text=PowerDMARC%20Pricing%2C%20Features%2C%20and%20Reviews,%C2%B7%20Up%20to%202%2C000%2C000)[

![Favicon](https://www.google.com/s2/favicons?domain=https://autospf.com&sz=32)

A Guide to Choosing the Right SPF Flattening Tool for Your Organization - AutoSPF - Automatic SPF flattening

https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/

](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=,MSP%2FMSSP%3A%20Quote%20on%20Request)[

![Favicon](https://www.google.com/s2/favicons?domain=https://knowledge.ondmarc.redsift.com&sz=32)

How does Dynamic SPF work? | OnDMARC Help Center - Red Sift

https://knowledge.ondmarc.redsift.com/en/articles/3822217-how-does-dynamic-spf-work

](https://knowledge.ondmarc.redsift.com/en/articles/3822217-how-does-dynamic-spf-work#:~:text=How%20does%20Dynamic%20SPF%20work%3F,includes%20honouring%20their%20published%20TTL)[

![Favicon](https://www.google.com/s2/favicons?domain=https://autospf.com&sz=32)

A Guide to Choosing the Right SPF Flattening Tool for Your Organization - AutoSPF - Automatic SPF flattening

https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/

](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=What%20to%20Expect%3F)[

![Favicon](https://www.google.com/s2/favicons?domain=https://autospf.com&sz=32)

A Guide to Choosing the Right SPF Flattening Tool for Your Organization - AutoSPF - Automatic SPF flattening

https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/

](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=,business%20owners%20and%20startups%3A%20%248%2Fmonth)[

![Favicon](https://www.google.com/s2/favicons?domain=https://mxtoolbox.com&sz=32)

Delivery Center Plus

https://mxtoolbox.com/c/products/deliverycenterplus?source=FEAT-SPFF

](https://mxtoolbox.com/c/products/deliverycenterplus?source=FEAT-SPFF#:~:text=Mxtoolbox%20Delivery%20Center%20Plus%20has,for%20up%20to%205%20Domains)[

![Favicon](https://www.google.com/s2/favicons?domain=https://mxtoolbox.com&sz=32)

Delivery Center Plus

https://mxtoolbox.com/c/products/deliverycenterplus?source=FEAT-SPFF

](https://mxtoolbox.com/c/products/deliverycenterplus?source=FEAT-SPFF#:~:text=great%20features%20of%20Delivery%20Center%2C,for%20up%20to%205%20Domains)[

![Favicon](https://www.google.com/s2/favicons?domain=https://mxtoolbox.com&sz=32)

SPF Flattening Tool - SPF Flattening Service - Flatten your SPF Record - MxToolbox

https://mxtoolbox.com/c/products/features/spf-flattening

](https://mxtoolbox.com/c/products/features/spf-flattening#:~:text=MxToolbox%20SPF%20Flattening%20Tool%20provides,reconstructing%20your%20SPF%20record%20automatically)[

![Favicon](https://www.google.com/s2/favicons?domain=https://mxtoolbox.com&sz=32)

SPF Flattening Tool - SPF Flattening Service - Flatten your SPF Record - MxToolbox

https://mxtoolbox.com/c/products/features/spf-flattening

](https://mxtoolbox.com/c/products/features/spf-flattening#:~:text=Image%3A%20SPF%20Flattening%20Simplifies%20the,SPF%20Record)[

![Favicon](https://www.google.com/s2/favicons?domain=https://easydmarc.com&sz=32)

EasySPF | Automatic SPF Configuration | EasyDMARC

https://easydmarc.com/tools/easy-spf

](https://easydmarc.com/tools/easy-spf#:~:text=EasySPF%20uses%20a%20dynamic%20SPF,record%20is%20up%20to%20date)[

![Favicon](https://www.google.com/s2/favicons?domain=https://mxtoolbox.com&sz=32)

Delivery Center Plus

https://mxtoolbox.com/c/products/deliverycenterplus?source=FEAT-SPFF

](https://mxtoolbox.com/c/products/deliverycenterplus?source=FEAT-SPFF#:~:text=,Domain%28s%29%3A%205)[

![Favicon](https://www.google.com/s2/favicons?domain=https://mxtoolbox.com&sz=32)

Delivery Center Plus

https://mxtoolbox.com/c/products/deliverycenterplus?source=FEAT-SPFF

](https://mxtoolbox.com/c/products/deliverycenterplus?source=FEAT-SPFF#:~:text=)[

![Favicon](https://www.google.com/s2/favicons?domain=https://mxtoolbox.com&sz=32)

SPF Flattening Tool - SPF Flattening Service - Flatten your SPF Record - MxToolbox

https://mxtoolbox.com/c/products/features/spf-flattening

](https://mxtoolbox.com/c/products/features/spf-flattening#:~:text=MxToolbox%27s%20SPF%20Flattening%20service%20gives,and%20keep%20your%20email%20going)[

![Favicon](https://www.google.com/s2/favicons?domain=https://mxtoolbox.com&sz=32)

SPF Flattening Tool - SPF Flattening Service - Flatten your SPF Record - MxToolbox

https://mxtoolbox.com/c/products/features/spf-flattening

](https://mxtoolbox.com/c/products/features/spf-flattening#:~:text=manage%20complex%20SPF%20configurations,reconstructing%20your%20SPF%20record%20automatically)[

![Favicon](https://www.google.com/s2/favicons?domain=https://www.valimail.com&sz=32)

SPF Flattening: What It Is & Why It Doesn’t Work

https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/

](https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/#:~:text=The%20alternative%20to%20SPF%20flattening%3A,Valimail)[

![Favicon](https://www.google.com/s2/favicons?domain=https://www.reddit.com&sz=32)

SPF Flattening Recommendations : r/sysadmin

https://www.reddit.com/r/sysadmin/comments/m1f17s/spf_flattening_recommendations/

](https://www.reddit.com/r/sysadmin/comments/m1f17s/spf_flattening_recommendations/#:~:text=like%3A)[

![Favicon](https://www.google.com/s2/favicons?domain=https://www.valimail.com&sz=32)

SPF Flattening: What It Is & Why It Doesn’t Work

https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/

](https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/#:~:text=match%20at%20L357%20Valimail%20Instant,to%20each%20mail%20server%20request)[

![Favicon](https://www.google.com/s2/favicons?domain=https://www.valimail.com&sz=32)

SPF Flattening: What It Is & Why It Doesn’t Work

https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/

](https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/#:~:text=match%20at%20L365%20Instant%20SPF,and%20stay%20at%20continuous%20enforcement)[

![Favicon](https://www.google.com/s2/favicons?domain=https://www.valimail.com&sz=32)

SPF Flattening: What It Is & Why It Doesn’t Work

https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/

](https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/#:~:text=Valimail%20Instant%20SPF%20is%20the,to%20each%20mail%20server%20request)[

![Favicon](https://www.google.com/s2/favicons?domain=https://www.valimail.com&sz=32)

SPF Flattening: What It Is & Why It Doesn’t Work

https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/

](https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/#:~:text=Automated%20solutions%20like%20Valimail%20Instant,advantages%20over%20manual%20SPF%20flattening)[

![Favicon](https://www.google.com/s2/favicons?domain=https://www.valimail.com&sz=32)

SPF Flattening: What It Is & Why It Doesn’t Work

https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/

](https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/#:~:text=SPF%20Flattening%3A%20What%20It%20Is,Why%20It%20Doesn%E2%80%99t%20Work)[

![Favicon](https://www.google.com/s2/favicons?domain=https://dmarcadvisor.com&sz=32)

SPF Management: Accurate and Up to Date SPF Records

https://dmarcadvisor.com/spf-management/

](https://dmarcadvisor.com/spf-management/#:~:text=SPF%20Management%3A%20Accurate%20and%20Up,SPF%20record%20when%20changes%20occur)[

![Favicon](https://www.google.com/s2/favicons?domain=https://dmarcadvisor.com&sz=32)

SPF Flattening: Solve your Too Many DNS Lookups Now!

https://dmarcadvisor.com/spf-flattening/

](https://dmarcadvisor.com/spf-flattening/#:~:text=SPF%20Flattening%20by%20DMARC%20Advisor,maximum%20control%20with%20minimum%20risk)[

![Favicon](https://www.google.com/s2/favicons?domain=https://www.reddit.com&sz=32)

SPF Flattening Recommendations : r/sysadmin

https://www.reddit.com/r/sysadmin/comments/m1f17s/spf_flattening_recommendations/

](https://www.reddit.com/r/sysadmin/comments/m1f17s/spf_flattening_recommendations/#:~:text=records%20accordingly)[

![Favicon](https://www.google.com/s2/favicons?domain=https://dmarcian.com&sz=32)

SPF Best Practices: Avoiding SPF Record Flattening - Dmarcian

https://dmarcian.com/spf-best-practices/

](https://dmarcian.com/spf-best-practices/#:~:text=SPF%20Best%20Practices%3A%20Avoiding%20SPF,your%20SPF%20records%20using)[

![Favicon](https://www.google.com/s2/favicons?domain=https://dmarcly.com&sz=32)

Free SPF Record Checker - DMARCLY

https://dmarcly.com/tools/spf-record-checker

](https://dmarcly.com/tools/spf-record-checker#:~:text=Free%20SPF%20Record%20Checker%20,button%2C%20then%20scroll%20down)[

![Favicon](https://www.google.com/s2/favicons?domain=https://dmarcduty.com&sz=32)

Free SPF Flattening Tool - DmarcDuty

https://dmarcduty.com/spf-flattening/step1

](https://dmarcduty.com/spf-flattening/step1#:~:text=Our%20free%20SPF%20flattening%20tool,)[

![Favicon](https://www.google.com/s2/favicons?domain=https://www.reddit.com&sz=32)

SPF Flattening Recommendations : r/sysadmin - Reddit

https://www.reddit.com/r/sysadmin/comments/m1f17s/spf_flattening_recommendations/

](https://www.reddit.com/r/sysadmin/comments/m1f17s/spf_flattening_recommendations/#:~:text=SPF%20Flattening%20Recommendations%20%3A%20r%2Fsysadmin,make%20your%20own%20flattening%20service)[

![Favicon](https://www.google.com/s2/favicons?domain=https://domainadmintools.com&sz=32)

Easydmarc vs Dmarcly - Domain Admin Tools

https://domainadmintools.com/easydmarc-vs-dmarcly/

](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=Dmarcly%20does%20not%20have%20any,Enterprise%20at%20%24199%20per%20month)[

![Favicon](https://www.google.com/s2/favicons?domain=https://powerdmarc.com&sz=32)

Automatic SPF Flattening Tool: Remove Extra Lookups

https://powerdmarc.com/spf-rate-limit-permerror/

](https://powerdmarc.com/spf-rate-limit-permerror/#:~:text=A%20One,the%2010%20DNS%20lookup%20limit)[

![Favicon](https://www.google.com/s2/favicons?domain=https://powerdmarc.com&sz=32)

Automatic SPF Flattening Tool: Remove Extra Lookups

https://powerdmarc.com/spf-rate-limit-permerror/

](https://powerdmarc.com/spf-rate-limit-permerror/#:~:text=stay%20updated%20on%20uninformed%20changes,the%2010%20DNS%20lookup%20limit)[

![Favicon](https://www.google.com/s2/favicons?domain=https://www.valimail.com&sz=32)

SPF Flattening: What It Is & Why It Doesn’t Work

https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/

](https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/#:~:text=Valimail%E2%80%99s%20solution%20is%20called%20Valimail,Valimail%20Enforce%20includes)[

![Favicon](https://www.google.com/s2/favicons?domain=https://domainadmintools.com&sz=32)

Easydmarc vs Dmarcly - Domain Admin Tools

https://domainadmintools.com/easydmarc-vs-dmarcly/

](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=Easydmarc%20has%20a%20FREE%20plan,cheaper%20if%20you%20go%20yearly)[

![Favicon](https://www.google.com/s2/favicons?domain=https://autospf.com&sz=32)

A Guide to Choosing the Right SPF Flattening Tool for Your Organization - AutoSPF - Automatic SPF flattening

https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/

](https://autospf.com/blog/guide-to-choosing-right-spf-flattening-tool-for-your-organization/#:~:text=,ideal%20for%20enterprise%20organizations%3A%20%24387%2Fmonth)[

![Favicon](https://www.google.com/s2/favicons?domain=https://autospf.com&sz=32)

- AutoSPF - Automatic SPF flattening

https://autospf.com/

](https://autospf.com/#:~:text=No%20email%20sending%20limits%2C%20no,based%20pricing)[

![Favicon](https://www.google.com/s2/favicons?domain=https://autospf.com&sz=32)

- AutoSPF - Automatic SPF flattening

https://autospf.com/

](https://autospf.com/#:~:text=No%20Extra%20Suite%20of%20Tools,Needed)

All Sources

[

![Favicon](https://www.google.com/s2/favicons?domain=https://dmarcreport.com&sz=32)

dmarcreport

](https://dmarcreport.com/flatten-spf-files-to-ensure-email-deliverability/#:~:text=AutoSPF%20automatically%20flattens%20SPF%20records%2C,process%20is%20highly%20automated%2C%20inherently)[

![Favicon](https://www.google.com/s2/favicons?domain=https://autospf.com&sz=32)

autospf

](https://autospf.com/#:~:text=We%20automate%20regular%20checks%20for,comes%20to%20your%20technology%20stack)[

![Favicon](https://www.google.com/s2/favicons?domain=https://www.reddit.com&sz=32)

reddit

](https://www.reddit.com/r/sysadmin/comments/m1f17s/spf_flattening_recommendations/#:~:text=There%20are%20two%20types%20of,flattening%20service)[

![Favicon](https://www.google.com/s2/favicons?domain=https://dmarcly.com&sz=32)

dmarcly

](https://dmarcly.com/blog/what-is-automatic-dynamic-spf-record-flattening-and-how-it-fixes-the-spf-permerror-too-many-dns-lookups-issue#:~:text=DMARCLY%27s%20Safe%20SPF%20feature%20implements,the%20DNS%20settings%20manually%20anymore)[

![Favicon](https://www.google.com/s2/favicons?domain=https://domainadmintools.com&sz=32)

domainadmintools

](https://domainadmintools.com/easydmarc-vs-dmarcly/#:~:text=you%20to%20manage%20unlimited%20domains,cheaper%20if%20you%20go%20yearly)[

![Favicon](https://www.google.com/s2/favicons?domain=https://easydmarc.com&sz=32)

easydmarc

](https://easydmarc.com/easy-spf-too-many-dns-lookups#:~:text=Add%20a%C2%A0single%20include%C2%A0in%20your%20SPF,and%20EasyDMARC%C2%A0takes%20care%C2%A0of%20the%20rest)[

![Favicon](https://www.google.com/s2/favicons?domain=https://support.openprovider.eu&sz=32)

support.openprovider

](https://support.openprovider.eu/hc/en-us/articles/5294664442386--EasyDMARC-Frequently-Asked-Questions#:~:text=We%20strongly%20recommend%20using%20CNAME,DNS%20zone%20settings%20every%20time)[

![Favicon](https://www.google.com/s2/favicons?domain=https://powerdmarc.com&sz=32)

powerdmarc

](https://powerdmarc.com/spf-rate-limit-permerror/#:~:text=Your%20Dynamic%20and%20Hassle,Flattening%20tool)[

![Favicon](https://www.google.com/s2/favicons?domain=https://www.softwaresuggest.com&sz=32)

softwaresuggest

](https://www.softwaresuggest.com/powerdmarc#:~:text=PowerDMARC%20Pricing%2C%20Features%2C%20and%20Reviews,%C2%B7%20Up%20to%202%2C000%2C000)[

![Favicon](https://www.google.com/s2/favicons?domain=https://knowledge.ondmarc.redsift.com&sz=32)

knowledg...c.redsift

](https://knowledge.ondmarc.redsift.com/en/articles/3822217-how-does-dynamic-spf-work#:~:text=How%20does%20Dynamic%20SPF%20work%3F,includes%20honouring%20their%20published%20TTL)[

![Favicon](https://www.google.com/s2/favicons?domain=https://mxtoolbox.com&sz=32)

mxtoolbox

](https://mxtoolbox.com/c/products/deliverycenterplus?source=FEAT-SPFF#:~:text=Mxtoolbox%20Delivery%20Center%20Plus%20has,for%20up%20to%205%20Domains)[

![Favicon](https://www.google.com/s2/favicons?domain=https://www.valimail.com&sz=32)

valimail

](https://www.valimail.com/blog/ive-got-99-problems-but-spf-aint-one/#:~:text=The%20alternative%20to%20SPF%20flattening%3A,Valimail)[

![Favicon](https://www.google.com/s2/favicons?domain=https://dmarcadvisor.com&sz=32)

dmarcadvisor

](https://dmarcadvisor.com/spf-management/#:~:text=SPF%20Management%3A%20Accurate%20and%20Up,SPF%20record%20when%20changes%20occur)[

![Favicon](https://www.google.com/s2/favicons?domain=https://dmarcian.com&sz=32)

dmarcian

](https://dmarcian.com/spf-best-practices/#:~:text=SPF%20Best%20Practices%3A%20Avoiding%20SPF,your%20SPF%20records%20using)[

![Favicon](https://www.google.com/s2/favicons?domain=https://dmarcduty.com&sz=32)

dmarcduty

](https://dmarcduty.com/spf-flattening/step1#:~:text=Our%20free%20SPF%20flattening%20tool,)





