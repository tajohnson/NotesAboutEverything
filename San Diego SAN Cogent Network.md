
Aruba Instant On 1960 12XGT 4SFP+ Switch
sw02: 199.89.3.245  - but do NOT need a public IP address.  The cloud interface can still work.
Internal address: 10.0.3.245

Aruba Instant On 1960 24G 2XGT 2SFP+ Switch  (Ipmi, other)
  sw02: 199.89.3.246   - but do NOT need a public IP address.  The cloud interface can still work.
  Internatl address: 10.0.3.246


Login: admin
Password:  <See "Secure Info">

Login: tj
Password: <See "Secure Info">

HTTPS management port 44391



Cogent Tech Support
 IP guru: elton



Letter to send to cogent if they don't fix the IRR records
https://bgp.he.net/net/199.89.2.0/24 should have a green check:

```
   

Hi Cogent NOC / Routing Team,

We recently brought up two new prefixes with Cogent as the originating ASN (**AS174**), and we’d like to ensure IRR/route-filtering compatibility across the internet.

Could you please create (or confirm creation of) **IRR route objects** for the following announcements:

- **199.89.2.0/24** — origin **AS174**
- **199.89.3.0/24** — origin **AS174**
    
These prefixes are part of our ARIN allocation **199.89.0.0/21** (registrant: Thomas A. Johnson Group, Inc.). We’ve already published **RPKI ROAs** authorizing AS174 for the two /24s.

If you prefer a specific IRR database (e.g., RADB/NTTCOM/ALTDB/etc.) or need an LOA template from us, please let me know what format you require and I’ll provide it immediately.

Thanks,

Thomas Johnson
MailRoute
tj@mailroute.net


```