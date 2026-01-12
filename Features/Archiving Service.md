# Archiving Service

## Status
- [ ] Figure out archiving approach

## Overview
- Start with O365 only (journal rules)
- Mail flow: o365 journal → postfix-archive → extract message/rfc822 → elasticsearch

## Domains Registered
- mailroutearchive.com
- mailroutearchive.cloud
- mrarchive.online
- mrarchive.cloud
- mrarchive.net

## Mail Flow Details

O365 journal rule sends email to a specific address (one per customer, perhaps?).
The email that's sent has a mime attachment message/rfc822 that contains the original email.
We need to extract that, and then deliver it to the user's actual mailbox.

### postfix-archive
Do this as a variation of the current postfix-deliver? maybe or maybe not.
Check to see if legit address - mailfrom is the admin email that's set, so can use that too:
```
Return-Path: <o365archiveadmin@terramar.net>
Delivered-To: o365archive@terramar.net
```

### postfix pipe delivery agent
Perl script that:
- Strips out message/rfc822 attachment
- Parses out necessary headers
- Adds it to elasticsearch

Work in progress on s02.lax in ~tj/archive/

## Storage Estimates (June 2019 numbers)

- Delivered (clean or spammy): 29,126,650 emails
- Total size: 4265913371927 bytes (~3.9TB of mail)

| Retention | Storage | Emails |
|-----------|---------|--------|
| 12 months | 46.8TB | 348M |
| 84 months (7 years) | 327.6TB | 2.44B |

Current elasticsearch for log searching: 1.25B records, ~0.85TB
