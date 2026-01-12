In this document, we present a detailed overview of MailRoute’s email security and hosted email service architecture, and discuss how it can be adapted to meet the needs of **BOLD** – a technology platform for job seekers and career services. 

MailRoute is a cloud-based email security company providing spam filtering, virus/malware protection, and reliable email routing, while BOLD is known for its suite of career tools (AI-powered resume builders, job search sites, networking platforms, etc.) that connect job seekers with recruiters. BOLD’s new initiative requires a dedicated email communication system between recruiters and job seekers, with enhanced control and integration capabilities. This report first explains how MailRoute’s existing service works in depth, then identifies points in the workflow where customizations or integrations could fulfill BOLD’s requirements.

**Assumptions:** The reader is generally familiar with email (e.g. concepts like mail servers and MX records) but may not be versed in advanced topics like globally load-balanced DNS, email authentication standards (SPF, DKIM, DMARC), or the nuances of spam filtering techniques. Therefore, we describe MailRoute’s architecture in business-technical terms, without diving into low-level email protocol details, and highlight how and where the system can be tailored for custom needs.

## MailRoute Service Architecture Overview

MailRoute operates as a secure email gateway in the cloud, sitting _between_ the internet and a customer’s mail servers. By updating the domain’s MX records to point to MailRoute, customers route their inbound email through MailRoute’s infrastructure for filtering and processing before it reaches the actual mailbox destination. Outbound emails can also be relayed via MailRoute’s servers for filtering and policy enforcement, if the customer chooses to use MailRoute as an outbound smart host.

### Global Distributed Infrastructure and Reliability

MailRoute’s network is designed for high availability and performance. The company maintains multiple geographically distributed data centers (at least three in the United States) which act as the “edge” of the service. Incoming email traffic is dynamically routed to the optimal data center based on factors like the sender/recipient location, server load, and network health. This global load-balancing ensures minimal latency (most messages are processed in under 2 seconds) and provides redundancy: each data center runs at no more than 33% capacity so that traffic can failover to others if needed. MailRoute confidently offers a 99.999% uptime service level (translating to only seconds of downtime allowed per week).

Within each data center, the architecture is highly redundant at every layer. There are multiple internet connections (from diverse providers and paths) feeding dual routers, which connect to multiple switches, ensuring no single network point of failure. Clusters of servers handle the email processing; load balancers in each location distribute connections to the healthiest server cluster and then to the optimal server within the cluster. Each server has dual network interfaces to separate switches for resilience.

Crucially, when an email message is received by MailRoute, it is immediately written to at least two different servers in the cluster in real-time. This _redundant array of independent servers_ approach ensures that even if one processing server fails mid-processing, a copy exists on another server so that no message is ever lost. All configuration and state data (like user settings, filter rules, etc.) are stored in replicated databases, so each data center has full knowledge and can operate independently if isolated. Security at the physical level is also enforced: data centers are secured with 24×7 monitoring, biometric access controls, and MailRoute’s equipment resides in locked cages accessible only to MailRoute personnel.

**Outbound Infrastructure:** For customers who use MailRoute for sending outbound mail, the service similarly routes outgoing messages through the nearest/best data center. Outgoing mail passes through health and policy checks, and MailRoute monitors sending patterns (using AI/ML techniques) to quickly detect anomalies such as compromised accounts or spam bursts. If an outgoing message triggers an issue (say, it looks like spam or malware), the system can flag it block the message transmission immediately. This ensures that a customer’s sending reputation is protected (preventing, for example, a compromised account from blasting spam to external recipients). Outbound messages are DKIM-signed by MailRoute on behalf of customers to ensure proper authentication and deliverability.

### Inbound Email Flow and Multi-Layer Filtering

When an outside sender transmits an email to a MailRoute-protected domain (such as one of BOLD’s domains), the message first hits MailRoute’s DNS, which directs it to the best data center as described. Within the chosen data center, the flow of inbound email processing can be broken into several stages:

1. **Connection and Sender Reputation Check (Edge Blocking):** Before fully accepting the email, MailRoute performs an adaptive screening on the incoming connection. The source IP address of the sending mail server is checked against dozens of real-time blacklists (RBLs) and reputation services (MailRoute uses around 40 different blacklist sources, both its own and third-party). If the sender’s IP has a very poor reputation (for instance, known to send only spam or part of a botnet), MailRoute will _drop_ the connection or reject the message outright at this stage. This prevents a flood of known spam or denial-of-service attacks from even entering the system, dramatically reducing load on deeper processing and protecting customers from obvious junk. This blocking happens at the SMTP handshake level, meaning MailRoute can refuse the message before it is accepted, which is efficient. Only connections that are not blatantly bad are allowed through to the next stage.
    
2. **Message Acceptance and Redundancy:** If the connection passes the IP reputation check, the MailRoute servers will accept the email for processing. As mentioned, the email is immediately written to multiple servers’ disk storage in the cluster for fault-tolerance. From this point on, the message is safe in MailRoute’s system, and the sending server is given an acknowledgement. Now the content of the email can be analyzed thoroughly.
    
3. **Content Filtering and Analysis:** MailRoute performs deep, multi-layer content inspection using a combination of proprietary and open-source technologies. This happens very quickly (usually within a couple of seconds or less), but in a structured pipeline:
    
    - **Email Structure & Header Analysis:** The system examines the email headers (the routing metadata and technical details of the message) for anomalies or signs of abuse. For example, it checks the sequence of `Received` headers to see the path the email took and if any headers are malformed or forged. It verifies standard fields for consistency and legitimacy (e.g. detecting if someone forged the sender domain, which might be cross-checked with authentication results like SPF or DKIM if available). MailRoute also looks for headers characteristic of bulk mail or known mailing list software vs. normal personal email, and headers that might indicate phishing (such as deceptive Reply-To addresses). A “bad header” rule exists to catch blatantly malformed headers or header exploits – if triggered, the message may be flagged for rejection or quarantine as unsafe.
        
    - **Allow/Block List Application:** Before diving into content, MailRoute applies user-defined **Allow Lists (whitelists)** and **Block Lists (blacklists)**. These are custom rules set by the customer (domain administrators or even end-users for their own mailbox) specifying senders or other criteria that should always be allowed or always blocked. The system checks the sender’s addresses against these lists:
        
        - _By Address or Domain:_ If the sender’s email address or domain is on an Allow List, MailRoute will bypass spam filtering for that message (though virus and malware scans still apply for safety). Conversely, if the sender is on a Block List, the message will be  quarantined as spam directly. There is an override hierarchy: specific addresses take precedence over domain-wide entries. For example, one could block all email from `@abc.com` but then allow a specific address `person@abc.com` – the more specific rule wins.
            
        - _Conditional Allow/Block:_ MailRoute supports sophisticated conditions for these lists. Administrators can restrict an allow rule to apply only when the message comes through certain servers or IP ranges. For instance, one can allow emails from `user@outlook.com` _only_ if the sending server’s reverse DNS ends in `.outlook.com` – ensuring the email truly originates from Outlook/Microsoft servers. IP ranges can be specified in CIDR notation as well (e.g. allow an entire subnet of partner company mail servers). This flexibility helps prevent spoofing: e.g., you might allow an address only when sent via a known mail relay.
            
        - _Header or Subject-Based Rules:_ Uniquely, MailRoute enables allowing or blocking messages based on content in specific email headers or in the subject line. An admin/user can define a substring or pattern to search for. For example, one could block any email with the subject containing “Make money fast” or allow emails with a header X-Important: YES. These patterns accept wildcards (`*`and `?`) for basic pattern matching. This can even be used to allow/block based on presence of a specific phrase in headers like `Authentication-Results` or `DKIM-Signature`. (In practice, this means an organization could craft a rule like “allow if the DKIM-Signature header contains `d=example.com`”, effectively allowing email with a particular valid DKIM signature. 
            
        - These Allow/Block list checks are a “blunt instrument” but very effective for custom policy needs – especially relevant for BOLD if, say, they want to **restrict communications to only certain domains or users**. We will revisit this in the customization section.
            
    - **Attachment Scanning and File Analysis:** MailRoute’s system pulls apart each email into its components. If the email has attachments (or is itself an attachment container like a ZIP file), those are extracted and decoded recursively. For example, if a .zip file contains a Word document which has an embedded macro, the filtering system will dig into all layers. All extracted files are scanned by **multiple antivirus engines (at least two different AV scanners)**. Using more than one AV engine increases catch rates for known malware, as one engine might detect something another misses. Additionally, MailRoute maintains a list of outright **banned file types** – file attachments that are commonly dangerous and have little legitimate purpose in business email (such as `.exe`, `.scr`, or other executable file formats). If an email contains a banned file type, that email will be flagged and handled as containing a prohibited attachment. The administrator alone can be given the option to release it from quarantine if it was a legitimate file, but by default these are stopped to protect the recipient’s environment. Compressed files (zip, rar, etc.) that contain banned file types inside are handled in the same way (the system will identify the contents after decompression).
        
    - **URL & Link Inspection:** A critical part of content filtering is scanning any URLs present in the email body or attachments. MailRoute checks every URL against an extensive database of known malicious or phishing websites. If a URL is a known bad link (e.g., linked to malware distribution or a phishing page), that will heavily weight the email as malicious. Furthermore, if the email uses URL shorteners or redirects (like bit.ly, t.co, etc.), MailRoute will resolve those to their final destination and then evaluate the final URL. This ensures that even if an email tries to hide a bad link behind a redirect, it will be caught. This thorough URL analysis protects users from clicking links that could lead to scams or malware.
        
    - **Content and Keyword Analysis:** The email’s body text and other components are scanned against MailRoute’s vast rule set of spam and phishing indicators. MailRoute leverages **hundreds of thousands of proprietary rules** looking for known spam phrases, behavioral patterns, and anomalies in how the email is constructed. Each rule, when triggered, contributes a certain score (positive or negative) to the email’s overall _spam score_. This approach is conceptually similar to how SpamAssassin (a well-known open-source spam filter) scores messages, though MailRoute’s ruleset is a unique blend of open-source foundations (like SpamAssassin) and extensive proprietary rules continuously maintained and updated by MailRoute. For example, there are rules that detect phrases commonly used in fraud (e.g. “transfer funds”, certain pharmaceutical keywords, etc.), rules that detect if the MIME structure is broken or suspicious, rules for if the message seems auto-generated by a bot rather than a standard email client, and so forth. Some rules might subtract points (for example, if the message is from a known good newsletter source or has a proper greeting, etc., which are signs of legitimacy). The system also incorporates sender reputation beyond just the RBL blocklists – for instance, if a sender’s domain or IP has been sending borderline spam, it might carry a low positive score to reflect slight distrust, whereas an IP with an excellent history might contribute a negative score (making it less likely to be flagged). MailRoute also tracks emerging threats and adapts the rules daily, or even hourly, as new spam campaigns and techniques appear. (MailRoute’s team uses spam trap data, customer feedback – e.g. emails forwarded to their abuse address – and threat intelligence to train and update rules on an ongoing basis.)
        
4. **Spam Scoring and Classification:** As the content filtering stage completes, all the rule scores are summed into a final **Spam Score** for the message. This score is compared against the customer’s configured thresholds. MailRoute allows each domain or user to adjust how sensitive the filter is via a spam score threshold setting. By default, MailRoute’s **recommended threshold is 7.0** – meaning any message scoring 7 points or higher is considered spam and will be intercepted (quarantined) rather than delivered. This default has been tuned to catch virtually all spam while keeping false positives very low. However, administrators can raise or lower this threshold if they find they want stricter filtering or more leniency.
    
    In addition to the main spam cutoff, MailRoute supports a secondary threshold for tagging “spammy” messages. For instance, an administrator could configure that any message scoring, say, 5 or above gets delivered **but** with an indicator (for example, adding “[Possible Spam]” to the subject line or adding a header in the email) to warn the recipient. This is optional and often only used by a subset of users who want to see borderline cases in their inbox. Most users simply rely on the quarantine for anything over the primary threshold and don’t use spam tagging. But the option exists to deliver-and-mark or even to deliver with no mark up to a certain point, then quarantine beyond that. The granularity is per user or domain policy.
    
    After scoring, MailRoute categorizes the message into one of several outcomes:
    
    - **Clean Mail:** If the spam score is below the defined threshold (e.g., below 7 by default and not caught by any allow/block rule that forces it one way or the other), and no other severe issues were found, the email is considered “clean.” It is authorized for delivery to the recipient’s mailbox. Clean mail has passed all checks – it’s not spam, not a virus, not a banned attachment, etc. Such mail will be forwarded on to the customer’s mail server or mailbox (see **Delivery** step below). Before delivery, MailRoute adds some headers to the email for transparency – for example, headers that indicate the spam score and filtering results. These can be useful for troubleshooting or for the end-user to see (via mail client) why a message was flagged as spammy or allowed. But the content of the email is not altered except optionally adding a tag in the subject if the tagging feature is used. The body of the email itself is never altered.
        
    - **Spam (Quarantined Mail):** If the spam score meets or exceeds the quarantine threshold set for that mailbox/domain, the email is deemed **spam** and is _not delivered_ to the user’s inbox. Instead, it is placed in the MailRoute **quarantine** – a secure holding area on MailRoute’s servers. Quarantining spam keeps inboxes clean while allowing the user or admin to review these messages later (in case of any false positives). MailRoute’s quarantine is robust: messages in quarantine are stored on redundant systems and retained for a default of 15 days. After 15 days (configurable in some cases), quarantined items expire and are automatically deleted, to conserve space and comply with privacy expectations. During that retention period, users can take action on them (more on quarantine management shortly). 
        
    - **Malware/Virus-Positive Mail:** If during content scanning the message (or any attachment) was found to contain a virus or malware, MailRoute will quarantine it in a special category. Such messages are typically _not_ delivered to the end user under any circumstances (since they are definitively malicious). They are held in quarantine, and the system can optionally send notifications. For instance, an alert to the intended recipient can be enabled saying "A message addressed to you was blocked due to a virus."  Administrators can review malware-quarantined items via the admin console. In no case will a virus-infected email be passed through to the user’s inbox; the only way to release it would be if an admin explicitly overrides it (not common, unless it’s a false positive situation). If a message identified as containing malware is released by an admin, it is sent in a new email as an attachment. The original email is in a password-protected ZIP file; the enclosing email contains a warning, and information on how to access the content of the original message.  This is to ensure that any recipient opening up this message is fully aware of the risks that they are assuming.
        
    - **Blocked File Type (Restricted Attachment):** If an attachment is of a banned file type (e.g., an `.exe` file or other executable), the system will intercept the message and quarantine the message similarly to a virus (since these file types are considered too risky to deliver. The recipient or admin would see in their quarantine that the email was blocked due to a prohibited attachment. They could then decide to release it (if they have the ability and trust the file after scanning it manually) or simply leave it. This feature is important for a secure environment – for example, BOLD may decide that job seekers should never be sending `.exe` files to recruiters, so this policy is inherently enforced.

- **Policy Fail / Bad Header:** If an email violates some protocol standards severely (e.g., malformed headers or something that looks like an exploit attempt on an email client), MailRoute can also quarantine or drop it under a “bad header” or policy-fail category. This is relatively rare and usually indicates either a maliciously crafted email or a badly misconfigured sender.
    

5. **Delivery of Clean Mail:** For messages that are deemed clean (not spam/virus) after filtering, MailRoute proceeds to deliver the email to its intended destination. The destination could be:
    
    - **Customer’s Own Mail Server:** In traditional filtering deployments (and for most current MailRoute customers), this means MailRoute connects to the customer’s mail server (e.g., an on-premises Exchange server, or the customer’s cloud email host like Microsoft 365 or Google Workspace) and relays the email to it. MailRoute will have been configured with the customer’s server addresses and does this forwarding transparently. From the end-user’s perspective, the mail just arrives in their mailbox as normal, after a slight detour through MailRoute’s system. Once MailRoute successfully hands off the email to the destination server, MailRoute **deletes its stored copies of the message** from the filtering servers. This is an important point: **MailRoute does not retain clean email permanently** – this ensures privacy. (The only exception is an optional feature called _Continuity_ where a temporary copy of recent delivered mail can be kept to provide backup email access in case the customer’s server goes down. But if enabled, that copy is stored in a separate system and only accessible to the customer’s authorized users via a special interface. By default, clean mail is not archived by MailRoute beyond short-term needs).
        
    - **MailRoute Hosted Mailbox:** MailRoute also offers a **hosted email service** – essentially functioning as the email provider for the domain. In this case, if the recipient’s address corresponds to a mailbox on MailRoute’s own servers, the clean message is delivered internally to MailRoute’s mail storage (rather than forwarded outward). MailRoute’s hosted email platform provides IMAP and SMTP services so that users can directly access their email hosted at MailRoute. For example, if BOLD chooses to have MailRoute host the mailboxes for recruiters and candidates, the email flow for a clean message would end with the message residing on MailRoute’s mail server waiting for the user to read (via an IMAP client or webmail interface). Hosted mail users get the same spam/virus filtering benefits transparently, and they can send outbound mail through MailRoute’s SMTP servers with authentication. (In fact, from a workflow perspective, the filtering system treats MailRoute-hosted recipients the same as external – except the “delivery” is local. All filtering is identical, so the security level remains high.)
        
    
    MailRoute’s philosophy is to act as a seamless gateway. The filtering process typically introduces negligible delay (fractions of a second to a couple seconds), so users usually don’t notice any lag in email delivery. The benefit is that by the time mail reaches the mailbox, it’s clean and safe.
    
6. **Quarantine and User Self-Service:** All messages that were intercepted (spam, viruses, etc.) end up in the user’s quarantine repository on MailRoute’s system. The quarantine is accessible through MailRoute’s online **Control Panel**. Each user (and domain admin) can log in to view a list of their quarantined messages, categorized by type (Spam, Virus, Restricted Attachment, etc.). From there, they have several options for each message:
    
    - **Release (Deliver) to Mailbox:** If a message was mistakenly flagged (a false positive), the user can **recover**it from quarantine. This triggers MailRoute to deliver that message to the user’s primary mailbox (as if it had passed filtering). The system also learns from this action so that future emails from them are less likely to be blocked
        
    - **Permanent Allow or Block:** The user or admin can directly allow-list or block-list a sender from the quarantine interface. For instance, they might see a quarantined mail from `jane@example.com` (which they wanted) and click “Allow sender” – that adds Jane’s address to the allow list and optionally delivers the message. Conversely, if they see an obviously junk email that wasn’t automatically blocked, they could click “Block sender,” which will ensure that sender (or domain) is henceforth blocked.
        
    - **Delete:** They can delete the quarantined message if it’s spam they never want to see. Even if they do nothing, as noted, MailRoute will automatically purge old quarantine items after ~15 days to keep the storage clean. So users don’t have to manually clean it out; it’s a rolling window of the last two weeks of caught mail.
        
    - **View Details:** The interface also allows viewing the full email message in a safe manner, so the user can make an informed decision without risking downloading a virus.
        
    
    In addition to the web interface, MailRoute can send email **Quarantine Summary** notifications at configurable intervals (commonly once per day). These summary emails list all new quarantined messages for that user since the last summary. Each listed message can have direct action links – e.g., “Deliver”, “Allow”, “Block” right from the summary. This is convenient for users so they don’t have to constantly check a separate portal; they can just scan the daily digest in their inbox and see if any legitimate message got caught. Administrators can set the schedule for these notifications (daily, weekly, weekdays only, etc., or turn them off if not wanted).
    
7. **Administrative Control Panel:** MailRoute provides a comprehensive web-based admin console (and parallel API) for managing the service. An organization’s email administrator can log in to configure domain-wide settings such as:
    
    - Spam sensitivity (the score thresholds mentioned),
        
    - Quarantine notification schedules,
        
    - Allow/Block lists at the domain level (e.g., block an entire domain for all users, or allow a partner domain for all users),
        
    - Attachment policies (e.g., add or remove file types from the banned list if desired),
        
    - User management (adding/removing mailboxes, aliases, distribution lists),
        
    - and many more granular settings.
        
    
    Individual mailbox owners (users) can also log in (with their credentials) to adjust personal settings like their own allow/block list, spam threshold (if allowed by policy), and view their own quarantine. There are role-based controls too – e.g., a domain admin can manage settings for all users, whereas a user sees only their own stuff.
    
    The admin interface also provides **real-time logging and reports**. For instance, there’s a searchable mail log where an admin can trace an email (to see if it hit the system, what happened to it, etc. in real time). This can be invaluable for support (e.g., “Did my user receive that email? Was it filtered? Why?”). I
    
    In terms of integration, everything available in the web interface is also accessible via **MailRoute’s API** – allowing programmatic management of the service (more on the API later).
    

### MailRoute’s Hosted Email Service

While historically MailRoute has specialized in filtering and then relaying clean mail to customers’ own servers, it now also offers full **Hosted Mailboxes**. In the hosted scenario, MailRoute not only filters the email but also stores it and serves as the primary mail server for the domain’s users. This means MailRoute provides IMAP access for incoming mail and SMTP for sending (with authentication). Users can use any standard email client (Outlook, Thunderbird, mobile mail apps, etc.) to connect to MailRoute’s mail servers to send/receive mail, or the organization can build a custom interface leveraging IMAP/SMTP.

The hosted mail infrastructure leverages the same robust data centers and server architecture described above. By design, hosting is multi-tenant but strongly isolated by customer – a Bold employee’s mailbox data resides on storage allocated to Bold’s tenant, logically separated from any other customer’s data. Access controls ensure that only Bold’s authorized admins/users can access their domain’s mailboxes. MailRoute is experienced in high-security environments (they handle government and enterprise clients), and can even restrict data residency to certain regions if required.

Key features of hosted mail include:

- **Large Mailboxes:** By default, each user mailbox comes with a generous storage quota (e.g., 50 GB per user is included). Administrators can negotiate higher quotas or pooled storage; additional storage is available at a per-GB monthly cost.
    
- **Web and Mobile Access:** Currently, MailRoute’s primary user access is via IMAP/SMTP with the user’s choice of email client. MailRoute also offers a “webmail” interface to the end-users mailbox at https://webmail.mailroute.net. Bold may prefer to integrate email access into its own platform interface. This can be done by using IMAP protocols behind the scenes or via MailRoute’s APIs (if extended to message access).
    
- **Integrated Filtering:** All inbound mail to these hosted mailboxes goes through the exact same filtering pipeline described earlier. So Bold’s recruiters and candidates would benefit from the full spam/virus protection without any additional setup. Outbound mail from these mailboxes can also be configured to route through MailRoute’s outgoing servers, which apply outbound filtering and DKIM signing automatically. This ensures, for example, that if a job seeker’s mailbox was somehow compromised or started sending spam, MailRoute could intercept that to protect Bold’s domain reputation.
    
- **Administrative API and Sync:** Provisioning mailboxes can be done through the MailRoute admin UI or automated via the MailRoute API. For instance, when a new job seeker or recruiter is added to Bold’s system, Bold could call MailRoute’s API to create a corresponding mailbox, set up initial password or authentication link, and define any custom rules for that user. MailRoute also supports directory synchronization (LDAP, Azure AD, Google Workspace sync), but in Bold’s case, it likely would use API calls since Bold’s user base is managed in its own application database. This API covers creating/deleting users, setting aliases, password resets, and so on, enabling tight integration with Bold’s user management workflows.
    

Overall, the hosted email service would allow Bold to offer email functionality (to facilitate recruiter-candidate communications) without having to run its own mail servers or worry about spam/malware – MailRoute provides a turnkey email platform with enterprise-grade security.

### Security, Privacy, and Compliance

MailRoute’s service is built with security best practices and compliance in mind, which is particularly relevant if Bold needs to ensure user data protection:

- **Data Privacy:** Clean emails that are delivered are not stored long-term on MailRoute’s filtering servers. Quarantined messages (which by nature might include some legitimate personal communications caught as false positives) are stored for a limited period (15 days by default) and then deleted. MailRoute staff do not access customer email content except for support/troubleshooting with proper authorization. The data centers and storage systems are encrypted and physically secure. 
    
- **Isolation:** Bold’s email domain and users would be set up under an account that is segregated from other customers. No data is shared or visible across customers. Administrators from other organizations cannot see Bold’s mail flow or logs, etc. MailRoute even offers configurations for government or regulated clients where data is only handled by U.S. personnel, meeting ITAR requirements, etc., which demonstrates the level of isolation possible.
    
- **Compliance Standards:** MailRoute adheres to several industry and government security standards and can assist customers in their compliance needs. The platform meets or aligns with **NIST 800-171, CMMC, DFARS, FedRAMP, ITAR, and HIPAA** requirements. For Bold, this means the email system would support high levels of security (encryption, access control, audit logging) out of the box, and if Bold’s business expands to handling potentially sensitive personal data, the infrastructure is already vetted against strong standards.
    
- **Audit Logging:** All administrative actions (like an admin releasing an email from quarantine, or changing a setting) are logged. MailRoute also logs user logins and similar events. If Bold needs to audit who accessed what or when, these logs can be provided. Additionally, the mail logs can serve as an audit of message activity (e.g., a record of all messages sent/received for a period, which might be needed for compliance or analytics ).
    
- **Encryption:** Traffic between MailRoute and external mail servers can be encrypted via TLS (MailRoute supports SMTP over TLS for inbound/outbound). If Bold’s users use IMAP/SMTP, those are secured via SSL/TLS as well. In essence, data is encrypted in transit, and at rest on the servers encryption is applied where feasible.
    

### Summary of MailRoute’s Capabilities

To summarize MailRoute’s core service in context:

- It functions as a cloud email firewall and mail server, providing **multi-layered filtering** that includes IP reputation blocking, virus/malware scanning with multiple engines, deep content analysis (spam/phishing filtering with a massive rule-set and URL inspection), and flexible policy rules for allowed/blocked senders or content.
    
- The filtering and delivery process is highly resilient and fast – leveraging a distributed network for global performance and 5-nines reliability.
    
- Users and admins have tools to adjust the filtering to their needs (safe sender lists, custom rules, spam sensitivity settings) and to retrieve any messages that were quarantined, thus balancing security with control.
    
- MailRoute can act as a transparent proxy to protect an existing mail server, or it can serve as the primary mail host with full IMAP/SMTP service for users.
    
- All these functions are accessible via an API in addition to the web interface, meaning integration and automation with other systems (like Bold’s platform) is readily achievable.
    

With this solid foundation explained, we can now explore how Bold’s specific requirements can be met by MailRoute’s off-the-shelf offering and where additional customization or integration work may be applied to create a tailored solution.

## Customization & Integration for BOLD’s Requirements

BOLD’s use case involves facilitating communication between recruiters and job seekers through email, but with constraints and value-adds that a typical open email system might not provide. They want a controlled email environment, presumably under their domain (or a dedicated domain), where:

- Recruiters and candidates can only use this system for their communications (not a general-purpose email like Gmail).
    
- BOLD’s platform can access the content of these communications to trigger certain actions or workflows (for example, detect when a candidate sends a resume or when a recruiter requests more information, etc.).
    
- The communication should remain secure and free from spam, and possibly restricted so that external parties cannot contact these emails (to prevent abuse or off-platform communication).
    
- BOLD might also want to monitor or categorize the messages (for customer service quality, data insights, or automating parts of the recruiting process).
    

MailRoute’s platform, as described, already covers the security aspect thoroughly (spam/virus filtering). Additionally, it provides a robust email hosting capability. With some custom development and configuration, MailRoute can indeed cater to the integration and control needs. Below, we outline two approaches for BOLD and detail the integration points:

### 1. **“Off-the-Shelf” Solution (Minimal Customization)**

In the simplest deployment, BOLD could utilize MailRoute’s existing services with standard configuration:

- **MailRoute as Email Provider:** BOLD would get a dedicated email domain (for example, if BOLD’s main domain is bold.com, they might use a subdomain like `messaging.bold.com` or similar for this service, or even addresses like `recruiter@bold.com` managed through MailRoute). Mailboxes for each recruiter and each job seeker who needs to use the system would be created on MailRoute’s hosted service.
    
- **Provisioning & Authentication:** Using MailRoute’s API, BOLD’s system can create and delete mailboxes as users sign up or leave. BOLD could sync user login credentials or use a single sign-on approach – for instance, job seekers and recruiters could log into the BOLD platform and then access their MailRoute-provided mailbox without a separate login (this could be achieved via SSO/SAML if needed, as MailRoute supports SAML2.0 and OAuth for Microsoft/Google accounts, etc.)
    
- **Email Access:** Users (recruiters and candidates) could access their emails either via any email client (using the IMAP/SMTP credentials).  Or BOLD could build an in-app messaging interface. For example, a recruiter logged into BOLD’s website might have a “Messages” section where the web application calls MailRoute’s IMAP service (through an API or IMAP library) to fetch their messages and display them. When the recruiter sends a message to a candidate, the BOLD application could send it via SMTP through MailRoute. This way, users stay within BOLD’s UI but the underlying email is handled by MailRoute.
    
- **Filtering & Security:** All incoming messages from either side (or from external sources) would be filtered. In the scenario that the recruiters and job seekers only know each other’s BOLD-provided email addresses, any email coming into those addresses will be scanned. Spam is likely not a big issue here since the addresses might not be widely known – but if a spammer did find them, MailRoute would filter it out. More importantly, if, say, a job seeker’s personal email was compromised and started sending malicious attachments to the recruiter’s BOLD address, MailRoute’s virus and attachment filters would protect the recruiter.
    
- **Policy Enforcement:** Without any custom development, BOLD can already use MailRoute’s **allow/block rules**to enforce some communication restrictions. For instance, suppose recruiters should only receive emails from the specific candidates they are working with and vice versa, one could configure allow-lists accordingly. However, managing that at scale (dynamically updating allow lists per user pairing) might be cumbersome through the UI alone. Instead, via the API, BOLD could programmatically update allow/block entries: e.g., when a recruiter and a candidate are matched in the platform, automatically allow that candidate’s address to send to the recruiter’s address (and/or simply give both parties email addresses on the same domain and allow all internal communication, but block or quarantine any external senders except for specific exceptions). This leverages MailRoute’s existing policy engine with no new engineering, other than API scripting on Bold’s side.

- **Access to Mail Content:** If BOLD’s application needs to display email messages to users or extract message metadata/content for workflow automation (e.g., triggering an event when a resume is received), it can do so using standard IMAP protocols. Since MailRoute’s hosted mail platform is fully IMAP/SMTP-compliant, any application or service (including Bold’s backend) can securely connect to retrieve messages, download attachments, or mark messages as read. IMAP allows rich folder management, flagging, searching, and partial fetches (e.g., headers only or limited preview), so BOLD can implement a user experience tailored to its workflow. If further integration is needed, MailRoute can expose additional API endpoints for metadata access in the future, but for most use cases, IMAP is sufficient and standards-compliant.
    
- **API-Driven Provisioning and Reporting:** BOLD can fully automate mailbox/user creation, deletion, password resets, setting allow/block rules, and other configuration by calling MailRoute’s documented API endpoints. Anything the MailRoute admin UI can do is available via API, making it straightforward to integrate MailRoute as a background “email engine” for the BOLD platform.
    

**Pros of This Approach:**

- Fast to implement – leverages existing features.
    
- No custom MailRoute development needed beyond standard onboarding.
    
- High level of security and policy enforcement with minimal friction.
    
- All user-facing email access (read/send) can be controlled by BOLD.
    
- BOLD can monitor or ingest emails using IMAP or API as required.
    

**Cons / Limitations:**

- If BOLD wants to trigger real-time webhooks or events the moment an email is received or sent (rather than polling via IMAP), some custom integration would be needed (see next section).
    
- Fine-grained message categorization or content redaction/extraction at filtering time would require new MailRoute functionality or tighter integration (see Custom Options).
    

---

### 2. **Custom-Integrated Solution (with Event Hooks, Categorization, and Deep API)**

If BOLD’s needs go beyond what can be done through user-facing IMAP access (for example, if BOLD wants immediate notification of certain types of emails, needs to process/route messages differently based on content, or wants advanced reporting or custom retention flows), MailRoute’s architecture allows several **custom integration points**:

#### A. **Event Webhooks (Email Activity Notifications)**

MailRoute’s filtering platform can be extended to support real-time event notifications. For instance:

- When an email is received for a hosted mailbox (or after it passes filtering), MailRoute’s backend can generate a webhook event to a configurable endpoint at BOLD.
    
- The event payload can include metadata (sender, recipient, subject, timestamps, etc.), spam/virus scan results, and (optionally) the entire message or a link to fetch the full message/attachment (if the payload is too large).
    
- This allows BOLD’s platform to react immediately to key events: e.g., “Job Seeker X sent resume to Recruiter Y,” “Recruiter replied to Candidate,” etc.
    
- Webhook delivery can be made reliable by queueing events and retrying on failure. BOLD can design its own event handler to process, log, or trigger further workflows.
    
- MailRoute’s platform already uses Celery and distributed queues for internal event processing, so extending this model to customer webhooks is straightforward.
    

#### B. **Custom Message Categorization / Tagging**

MailRoute’s rule engine can be expanded to classify messages by custom logic defined by BOLD, such as:

- Tagging emails as “Application Intent,” “Reply Intent,” “Resume Submission,” or any other workflow category based on sender, subject keywords, attachments, etc.
    
- These tags can be written as special headers (X-Bold-Intent: Apply), metadata fields, or used to drive webhook events.
    
- Custom regex, keyword detection, or even NLP models can be deployed within MailRoute’s filtering pipeline, as this is already how proprietary and customer-specific rules are managed.
    

#### C. **Custom Quarantine / Moderation Flows**

If BOLD wants messages to be held for admin review, moderation, or delayed delivery based on content or sender (for example, hold all messages with certain attachments until scanned or approved), MailRoute can:

- Quarantine messages under custom tags (e.g., “Pending Moderation”), not just spam/virus.
    
- Expose API endpoints for BOLD’s admins or workflows to review and approve/release these messages.  Alternatively, MailRoute could provide a Custom UI for this feature within the the MailRoute Control Panel, as a custom development option.

    

#### D. **Custom API for Message Content Retrieval / Redaction**

If BOLD requires programmatic access to message bodies, attachments, or even redacted versions (for privacy/legal reasons), MailRoute can build a tailored API layer that:

- Exposes specific messages or message content to authorized BOLD services.
    
- Optionally performs inline redaction or masking of sensitive fields before transmission to BOLD.
    
- Allows searching, metadata extraction, or attachment handling for specific workflow triggers.
    

#### E. **Directory Integration and Authentication**

Beyond basic API provisioning, MailRoute can integrate with BOLD’s existing user directory (LDAP, Azure AD, Google Workspace, SAML2, etc.) for seamless account sync, SSO, or delegated authentication as needed.



---

## **Summary Table: Off-the-Shelf vs. Custom Integration**

| Feature/Requirement              | Off-the-Shelf | With Customization |
| -------------------------------- | ------------- | ------------------ |
| Spam/virus filtering             | ✔️            | ✔️                 |
| Hosted mailbox (IMAP/SMTP)       | ✔️            | ✔️                 |
| API provisioning                 | ✔️            | ✔️                 |
| Policy enforcement (allow/block) | ✔️            | ✔️                 |
| Directory sync / SSO             | ✔️            | ✔️                 |
| Webhook/event notifications      | —             | ✔️ (custom dev)    |
| Real-time categorization         | —             | ✔️ (custom dev)    |
| Moderation/delayed delivery      | —             | ✔️ (custom dev)    |
| Deep content API/redaction       | —             | ✔️ (custom dev)    |

## **Pricing Considerations**

- **Standard Offering:**
    
    - Per-user/month cost for filtering + hosted mailbox (e.g., $X/user/mo, includes 50GB/user, additional storage billed per GB as needed).
	    - Adjusted pricing may apply for different (higher or lower) storage quotas)
        
    - Full access to filtering, quarantine, and admin APIs.
        
- **Custom Integration:**
    
    - One-time development cost for webhook/event system, custom rules, API extensions, etc.
        
    - Ongoing per-user/month for hosted service + maintenance of integration.
        
    - Per-event or per-API-call charge if BOLD expects very high webhook/API usage, (eg. beyond the typical use of API for account management)
        

## **Conclusion**

MailRoute provides a proven, secure, and scalable email filtering and hosting platform with all the APIs and admin tooling needed for modern SaaS integration. For BOLD, most needs can be met with the existing offering – including account automation, policy control, and in-app mail access using industry standards. Where BOLD requires deeper workflow automation (instant notifications, custom tagging, advanced moderation), MailRoute’s architecture and API-driven backend are well-suited for custom development, with clear integration points for webhook/event processing and custom business logic. Security, compliance, and privacy are enforced at every layer. MailRoute can support BOLD’s initiative as a robust communication backbone, either as a turnkey solution or as a highly-integrated part of BOLD’s broader recruiting ecosystem.


