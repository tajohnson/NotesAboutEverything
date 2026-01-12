
The current way that sauserpref is done for whitelist_header, etc is incredibly inefficient - takes a bunch of queries, and generall sucks.

I've built a new wblist system that builds on the standard wblist system.  I know it's a bunch of changes, but it will really improve things from both a functional standpoint, and perhaps most importantly, encourage users to not just blindly whitelist senders - they would be encouraged to require dmarc, or apply other checks.



### Allow rules:

If no options are selected, allow based on email address only
If "Require DMARC pass" is selected, DMARC must pass AND any other selected checks must pass
If "Require DMARC pass" is not selected, the email must pass any selected checks (if any), regardless of DMARC status
Additional checks (Header and Server) use OR logic if both are selected

### Block rules:

Each selected criterion (Sender, Header, Server) creates a separate blocking rule
No combination of checks for blocking rules


### Allow dialog:
```
[x] Allow  [ ] Block

Sender Email: [input@example.com]

Security Options:
[ ] Require DMARC pass

Additional Checks:
 <maybe this is a table, and you can edit or delete an entry?>
[ ] Check Header: [Header field] [Header value]
[ ] Check Header: [Header field] [Header value]

[ ] Check Server: [IP/Hostname]
[ ] Check Server: [IP/Hostname]
[ ] Check Server: [IP/Hostname]

Preview:
-------------------------------
Allow emails from sender@example.com
   if DMARC passes
   AND the sending server matches [IP/Hostname] OR the Subject header matches "abc"
-------------------------------

[ ] I understand the risks of allowing without additional checks 
    (only visible if no checks are selected)

[Cancel]  [Save]

```


### Block dialog:

```
[ ] Allow  [x] Block

Sender Email: [input@example.com]

Block by: (select one or more)
[x] Sender Email (as entered above)
[ ] Header
[ ] Server

[Header section, visible if Header is checked]
Header Name: [                    ]   Header Value: [                    ]
Header Name: [                    ]   Header Value: [                    ]

[Server section, visible if Server is checked]
Server IP/Hostname: [                    ]
Server IP/Hostname: [                    ]
Server IP/Hostname: [                    ]

Preview:
-------------------------------
New blocking rules:
1. Block all emails from sender@example.com that come from server x.x.x.x
2.Block all emails from sender@example.com that contain "ABC" in the "Subject" header.
-------------------------------

Note: Each selected option will create a separate blocking rule.

[Cancel]  [Save]

```

## 10. UI Considerations

- Use radio buttons for Allow/Block selection at the top
- For Allow rules:
  - Use checkboxes for DMARC and additional check options
  - Provide tooltips explaining each option
  - Show warning for email-only allow rules
- For Block rules:
  - Use checkboxes for each blocking criterion
  - Show/hide Header and Server input fields based on checkbox selection
  - Clearly indicate that each selection creates a separate rule
- Use color coding: green for Allow, red for Block
- When switching between Allow and Block, reset all options to avoid confusion
- Implement real-time preview updates as users modify settings
- Update the UI to allow users to input and edit the additional checks (DMARC requirement, header checks, server checks)
- Ensure the UI can parse and display the JSON-encoded additional checks when editing existing rules
- Update the preview functionality to include information about the additional checks
- Turn on the "require dmarc" option by default


## Logic:

For Allow rules:
| DMARC Required | Header Check | Server Check | Outcome |
|----------------|--------------|--------------|---------|
| ❌ | ❌ | ❌ | Allow based on email address only |
| ✅ | ❌ | ❌ | Allow only if DMARC passes |
| ❌ | ✅ | ❌ | Allow if header matches |
| ❌ | ❌ | ✅ | Allow if server matches |
| ❌ | ✅ | ✅ | Allow if header OR server matches |
| ✅ | ✅ | ❌ | Allow if DMARC passes AND header matches |
| ✅ | ❌ | ✅ | Allow if DMARC passes AND server matches |
| ✅ | ✅ | ✅ | Allow if DMARC passes AND (header matches OR server matches) |


For Block rules:
| Sender | Header Check | Server Check | Outcome |
|--------|--------------|--------------|---------|
| ✅ | ❌ | ❌ | Block based on email address |
| ❌ | ✅ | ❌ | Block if header matches |
| ❌ | ❌ | ✅ | Block if server matches |

Note: For Block rules, each selected option creates a separate blocking rule.



# Database changes

## Current DB Structure:

```
CREATE TABLE `amavisd_wblist` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `sid` int(11) NOT NULL,
  `wb` varchar(10) NOT NULL,
  `rid` varchar(36) DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `amavisd_wblist_sid_75f0b5c807fb559c_uniq` (`sid`,`rid`),
  KEY `rid` (`rid`),
  CONSTRAINT `amavisd_wblist_sid_ae57ba21_fk_amavisd_mailaddr_id` FOREIGN KEY (`sid`) REFERENCES `amavisd_mailaddr` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=15883533 DEFAULT CHARSET=utf8

```


## New DB Structure:

Note that the use of the UUID is going away - instead we are using the domain_id and email_account_id.  If it's a wblist for a user, both domain_id and email_account_id should be set.  If it's a domain-wide wblist, then email_account_id is null:

```
CREATE TABLE `amavisd_wblist_extended` (
  `id` int NOT NULL AUTO_INCREMENT,
  `sid` int NOT NULL,
  `wb` varchar(10) NOT NULL,
  `domain_id` int DEFAULT NULL,
  `email_account_id` int DEFAULT NULL,
  `additional_checks` text,
  PRIMARY KEY (`id`),
  UNIQUE KEY `unique_wblist_entry` (`sid`,`wb`,`domain_id`,`email_account_id`),
  KEY `idx_domain_id` (`domain_id`),
  KEY `idx_email_account_id` (`email_account_id`)
) ENGINE=InnoDB

```

**Note: I have additional_checks in here right now as a "text" field, but I think it should become a JSON field with the mysql8 upgrade - I will be changing that and testing and then will update this ticket**

### new wblist sql query in custom hook:

```
my $g_sql_wblist_extended_query_template = q{
  SELECT wbl.wb, ma.email,
    CASE
      WHEN wbl.email_account_id IS NOT NULL THEN 'user'
      WHEN wbl.domain_id IS NOT NULL THEN 'domain'
      ELSE 'global'
    END AS match_type,
    wbl.additional_checks
  FROM amavisd_wblist_extended wbl
  JOIN amavisd_mailaddr ma ON wbl.sid = ma.id
  WHERE ((wbl.email_account_id = ?) 
     OR (wbl.email_account_id IS NULL AND wbl.domain_id = ?)
     OR (wbl.email_account_id IS NULL AND wbl.domain_id IS NULL))
    AND ma.email IN (%s)
  ORDER BY 
    CASE 
      WHEN wbl.email_account_id IS NOT NULL THEN 1
      WHEN wbl.domain_id IS NOT NULL THEN 2
      ELSE 3
    END,
    ma.priority DESC
};


The %s in the query is replaced with a series of email lookups, from most to least specific:
email_lookups = [
    'user+ext@sub.example.com',
    'user@sub.example.com',
    '@sub.example.com',
    '@.sub.example.com',
    '@.example.com',
    '@.com',
    '@.'
]

```


## Additional Checks Column:

The additional_checks column will store JSON-encoded strings. Here's an example of how the data might be stored:

```
{
  "require_dmarc": true,
  "header_checks": {"name": "Subject", "value": "Important"},
  "server_checks": "192.168.1.100"
}


Or


{
  "header_checks": [
    {
      "name": "From",
      "value": "@example.com"
    }
  ],
  "server_checks": [
    "mail.example.com"
  ],
  "require_dmarc": true
}

Or


{
  "server_checks": [
    "192.168.1.0/24",
    "mail.example.com",
    "203.0.113.0/24"
  ],
  "header_checks": [
    {
      "name": "From",
      "value": "@trusted-sender.com"
    },
    {
      "name": "Subject",
      "value": "^\\[Important\\]"
    },
    {
      "name": "X-Custom-Header",
      "value": "SpecialValue"
    }
  ],
  "require_dmarc": true
}

```


One thing that is more complicated than the old situation is when a sender has a bunch of different sauserpref types of rules - they all have to be condensed into the one additional_checks field for that sender/recipient.


## Server checks

The server checks support two main types of checks:

- IP-based checks
  - Exact IPv4 match - Example: "192.168.1.1"
  - IPv4 subnet match in CIDR notation - Example: "192.168.1.0/24"

 - Reverse DNS (RDNS) based checks
    - Domain or subdomain match - Example: "example.com" or "mail.example.com"

For the UI, you might want to consider:
 - A dropdown or toggle to switch between "IP-based" and "RDNS-based" checks.
 - For IP-based checks:
    - An input field for the IP address or subnet
    - An optional field for subnet mask or CIDR notation
    - Validation to ensure the entered IP is valid
- For RDNS-based checks:
    - A simple text input for the domain
    - Validation to ensure it's a valid domain format
 
Clear explanations or tooltips for each type of check:
  - For IP checks: "Enter a single IP address or a subnet in CIDR notation"
  - For RDNS checks: "Enter a domain name to match against the sending server's reverse DNS"
  - Examples for each type of check to guide users.
  - A note that multiple server checks can be added, and the email will pass if it matches any one of the checks.

Consider adding a "Test" button that allows users to input a sample IP or domain to see if it would match their entered check.

Remember to emphasize that these checks are based on the sending server's properties, not the content of the email itself. They're useful for whitelisting or blacklisting based on the email's origin.

## Header checks

Support both simple string matching and some safe regex.

We figure out of it's safe regex to use with this little snippet:

```
sub c_is_safe_regex {
    my ($pattern) = @_;
    return 0 if length($pattern) > 1000;
    return 0 if $pattern =~ /\(\?[^:]|\\\d|\\g|\\K|\\\{/;  # Exclude advanced regex features
    return 0 if $pattern =~ /\{(\d+,\d+|\d+,|,\d+)\}/ && ($1 > 20 || $2 > 20);  # Limit quantifiers
    return 1 if $pattern =~ /[\^\$\*\+\?\[\]\(\)\{\}\|\\]/;  # Contains regex metacharacters
    return 0;  # Simple string
}
```

**Simple String Matching** - If the pattern doesn't contain regex metacharacters, it's treated as a simple string. The system checks if the header value contains this string (case-insensitive).

**Regex Matching** - If the pattern contains regex metacharacters, it's treated as a regex pattern. However, there are safety measures in place:
 - The pattern must be less than 1000 characters long.
 - Certain advanced regex features are excluded for safety.
 - There are limits on quantifiers to prevent excessive backtracking.




Here's a sort of cheat-sheet we could provide in a tooltip for users:


# Updated Regex Cheat Sheet for Header Checks

## Basic Matches
- `.` : Matches any single character
- `*` : Matches 0 or more of the preceding character
- `+` : Matches 1 or more of the preceding character
- `?` : Matches 0 or 1 of the preceding character
- `^` : Matches the start of the line
- `$` : Matches the end of the line

## Character Classes
- `[abc]` : Matches any single character in the set
- `[^abc]` : Matches any single character not in the set
- `[a-z]` : Matches any single character in the range

## Quantifiers
- `{n}` : Matches exactly n occurrences (max 20)
- `{n,m}` : Matches between n and m occurrences (max 20)

## Limitations
- Maximum pattern length: 1000 characters
- Quantifiers limited to max 20 repetitions
- No backreferences or advanced features

## Examples
- `Subject.*important` : Matches "Subject" followed by any characters and then "important"
- `^Re:` : Matches "Re:" at the start of the subject
- `[A-Z]{3}-\d{2}` : Matches three uppercase letters, a hyphen, and two digits

Note: 
1. Regex matching is case-insensitive in header checks.
2. If no metacharacters are used, the pattern is treated as a simple string match.




## Migrations

### Migrate amavisd_wblist to amavisd_wblist_extended
 - set domain_id, email_account_id fields.
- for a regular sender-only wblist entry, additional_checks is NULL.

### Migrate sauserpref checks
  - add appropriate stuff to "additional_checks" field for header and rcvd checks
  - language settings and score changes handled differently (see below about "Additional Filtering Checks")




# Additional Filtering Checks

We currently have a feature for "preferred languages", right?  And if an email comes through with a language that's not in the list then we mark it as language_unwanted in the scoring.

I've now added the option for "preferred countries", which does the same thing, but based on countries that relayed the email

And options for "languages blocked" and "countries blocked".

We've always had the option to have score adjustments - some are done based on choices by the user for things like phishing protection level, some are set when they set up preferred languages and some are done by superusers to modify scoring for a particular user or domain.  All those things have been stored in the sauserpref table.  We need to move this to a new table.

If there are language, country settings, or additional filtering settings there should be an additional field in amavisd_policy that is set to "Y" so we know we have to do an additional lookup (and we can skip it when it's not needed).

```

```











