MAIL_CONFIG=/etc/postfix-inbound mailqfmt | egrep  'callsource|kellex' | awk '{print $1}' | tr -d "*" | tr -d "\!" | postsuper -c /etc/postfix-inbound -H -



MAIL_CONFIG=/etc/postfix-inbound mailqfmt | egrep  "salesforce|noreply|constan|bounce|mcsv|rsgsv|mcdlv|prvs" | awk '{print $1}' | tr -d "*" | tr -d "\!" | postsuper -c /etc/postfix-inbound -h -





MAIL_CONFIG=/etc/postfix-inbound mailqfmt  | grep acm.org | awk '{print $1}' | tr -d '*' | tr -d '!' | postsuper -h -
MAIL_CONFIG=/etc/postfix-inbound mailqfmt | grep hq.acm.org | grep "\!"  | head -300 | tr -d "\!" | postsuper -H -





mailqfmt.pl | awk '{print $1}' | grep "\!"  | head -300 | tr -d "\!" | postsuper -H -


mailqfmt.pl | grep -i "menominee" | awk '{print $1}' | tr -d "*" | tr -d "\!" | postsuper -h -






mailqfmt.pl | grep -i "constant" | awk '{print $1}' | grep "\!" | tr -d "*" | tr -d "\!" |  head -500 |postsuper -H -
mailqfmt.pl | grep -i "MAILER" | awk '{print $1}' | grep "\!" | tr -d "*" | tr -d "\!" | head -500 | postsuper -H -
mailqfmt.pl | grep -i "bounce" | awk '{print $1}' | grep "\!" | tr -d "*" | tr -d "\!" |  head -500 |postsuper -H -
mailqfmt.pl | grep -i "menominee" | awk '{print $1}' | grep "\!" | tr -d "*" | tr -d "\!" |  head -500 |postsuper -H -

mailqfmt.pl | grep -i "atmc.net" | awk '{print $1}' | grep "\!" | tr -d "*" | tr -d "\!"  |postsuper -H -

mailqfmt.pl | grep -i "acm.org" | awk '{print $1}' | grep "\!" | tr -d "*" | tr -d "\!" |  head -500 |postsuper -H -


mailqfmt.pl | grep -i "acm.org" | awk '{print $1}' | grep "\!" | tr -d "*" | tr -d "\!" |  head -500 |postsuper -H -



mailqfmt.pl | grep -i "cppcorp.com" | awk '{print $1}' | grep "\!" | tr -d "*" | tr -d "\!" | postsuper -H -



mailqfmt-inbound  | grep bounce | awk '{print $1}' | tr -d '*' | tr -d '!' | postsuper -h -
mailqfmt-inbound  | grep acm | awk '{print $1}' | tr -d '*' | tr -d '!' | postsuper -h -



