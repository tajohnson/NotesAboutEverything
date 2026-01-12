# MTA-STS Support

We can support this - our customers would create these two dns records:

```
mta-sts.<domain.com>  CNAME   mta.mailroute.net.

_mta-sts.<domain.com> CNAME  mta_sts.mailroute.net.
```

And then we have this in our DNS:

```
mta.mailroute.net. 300 IN A <ip address of server hosting mta-sts record below>

mta_sts.mailroute.net.	60 IN TXT "v=STSv1; id=20250905T162108;"

```

And we host a text file https://mta.mailroute.net:
```
version: STSv1
mode: enforce
max_age: 31449600
mx: mail.mailroute.net

```

https://mxtoolbox.com/dmarc/details/mta-sts/what-is-mta-sts-record

#### IMPORTANT: check about https certificate!!!!  Is it okay to have it hosted by a cert on mailroute.net?
