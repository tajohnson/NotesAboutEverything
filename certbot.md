testing

# /opt/certbot/venv/bin/certbot certonly --dry-run --cert-name mailroute.net  --dns-rfc2136 --dns-rfc2136-credentials /etc/letsencrypt/rfc2136.ini  --agree-tos --email tj@terramar.net --preferred-challenges dns -d mailroute.net,*.mailroute.net,*.ssl.mailroute.net,*.int.mailroute.net,*.lax.mailroute.net,*.mia.mailroute.net,*.lax.int.mailroute.net,*.mia.int.mailroute.net --debug


mailroute.net
*.mailroute.net
*.ssl.mailroute.net
*.int.mailroute.net
*.lax.mailroute.net
*.mia.mailroute.net
*.lax.int.mailroute.net
*.mia.int.mailroute.net

dig +short @ns13.mailroute.net. txt _acme-challenge.lax.int.mailroute.net
dig +short @ns33.mailroute.net. txt _acme-challenge.lax.int.mailroute.net
dig +short @ns32.mailroute.net. txt _acme-challenge.lax.int.mailroute.net
dig +short @ns6.dnsmadeeasy.com. txt _acme-challenge.lax.int.mailroute.net
dig +short @ns31.mailroute.net. txt _acme-challenge.lax.int.mailroute.net
dig +short @ns12.mailroute.net. txt _acme-challenge.lax.int.mailroute.net
dig +short @ns5.dnsmadeeasy.com. txt _acme-challenge.lax.int.mailroute.net
dig +short @ns11.mailroute.net. txt _acme-challenge.lax.int.mailroute.net




Here's how to do a cert by hand, using webserver:


certbot certonly --cert-name mailroutecompliance.com --webroot --webroot-path /var/www/mailroutecompliance.com   --preferred-challenges http  --noninteractive --agree-tos  --email tj@mailroute.net -d mailroutecompliance.com


Here's how to do a cert by hand, using webserver: by DNS:

/opt/certbot/venv/bin/certbot certonly --cert-name mroute.net --dns-rfc2136 --dns-rfc2136-credentials /etc/letsencrypt/rfc2136.ini    --preferred-challenges dns  --noninteractive --agree-tos  --email tj@mailroute.net -d mroute.net,*.mroute.net

# by website, using web authentication

/opt/certbot/venv/bin/certbot certonly --cert-name mailroutecompliance.com --webroot --webroot-path /var/www/spam.xobeemail.com   --preferred-challenges http  --noninteractive --agree-tos  --email tj@mailroute.net -d spam.xobeemail.com



/opt/certbot/venv/bin/certbot certonly --cert-name spamconsole.e-companies.com --webroot --webroot-path /var/www/certbot   --preferred-challenges http  --noninteractive --agree-tos  --email tj@mailroute.net -d spamconsole.e-companies.com


IMPORTANT!!

need to add manual for the script that copies things over to the new servers!
/opt/certbot/venv/bin/certbot certonly --cert-name spamconsole.e-companies.com --webroot --webroot-path /var/www/certbot   --preferred-challenges http  --noninteractive --agree-tos  --email tj@mailroute.net -d spamconsole.e-companies.com


