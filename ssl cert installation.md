convert pfx:

# Export the private key
openssl pkcs12 -in aa.pfx -out aa.key -nocerts -nodes

# take out the encryption from the private key
openssl rsa -in aa.key -out aa_s.key



Convert pfx to key and cert

openssl pkcs12 -in [yourfile.pfx] -nocerts -out [keyfile-encrypted.key]
openssl pkcs12 -in [yourfile.pfx] -clcerts -nokeys -out [certificate.crt]


get info about mailserver certs:

(sleep 2; printf "quit\r\n") |
	openssl s_client -showcerts -starttls smtp -connect mail.mailroute.net:25 2>&1 |
	openssl crl2pkcs7 -nocrl -certfile /dev/stdin |
	openssl pkcs7 -print_certs -noout