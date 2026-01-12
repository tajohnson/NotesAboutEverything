
#### ssh tunnel to access admin interface

create a tunnel on es01.lax.mailroute.net to access port 443 on imap.mailroute.net
	ssh -L 9443:199.89.1.129:443 root@199.89.0.103

make sure there's a /etc/hosts entry for stalwart-admin.mailroute.net that points at 127.0.0.1

then access via browser:  https://stalwart-admin.mailroute.net:9443


License:

```
Dear Thomas Johnson,

Thank you for sponsoring **Stalwart Mail Server** through **Github**! We greatly appreciate your support.

Your license key for **[mailroute.net](http://mailroute.net/)** is now available for download at [https://license.stalw.art/license/1851](https://license.stalw.art/license/1851). You can download or print your invoice at [https://license.stalw.art/invoice/1389](https://license.stalw.art/invoice/1389).

We've created an account for you with the following credentials:

- Email: **[tj@mailroute.net](mailto:tj@mailroute.net)**
- Password: **xubqem-famfe3-Kewhep**

Please [change your password](https://license.stalw.art/change-password) after your first login.

Thank you again for your support!

Best regards, Stalwart Labs Team


```



Generate a password:
python -c "from argon2 import PasswordHasher; import getpass; print(PasswordHasher().hash(getpass.getpass('Enter password: ')))"


See "Secure Info" for passwords.
fallback password: admin


master user: tj





stalwart master password - can access anybody's mailbox:

	user: user@domain.com%master
	password: <see "Secure Info">
	
	(Use this to do IMAP migrations?)


for nginx reverse proxy:
dnf install nginx-mod-stream.x86_64


setting up foundationdb - running on es01, es02, es03, es04

	Install on es01
	fdbcli
		configure ssd
	install on others, one at a time using ansible, restart foundationdb on each
	fdbcli
		configure double
		


backup fdb database:
example:
```
# fdbbackup start -d file:///var/lib/foundationdb/backup/2025-12-14
# fdbbackup status


```
  # 


stalwart file macro:
	# SSL/TLS Certificate from a local file
	[certificate."default"]m
	cert = "%{file:/etc/letsencrypt/live/mail.example.org/fullchain.pem}%"

 Stalwart does not support watching for file changes so if the certificate changes you need to reload it by sending a GET request to /api/reload/certificate (the API is not yet documented).


Tip: force it to update webadmin if you can't get into webadmin:

curl -k -u admin:<adminpass> http://localhost:8080/api/update/webadmin



django passwords.
  for sha256_crypt, remove just "sha256_crypt" from the beginning, pass the rest


s3 bucket: mr-hostedmail
s3 bucket owner: hostedmail-rw  

access key: See "Secure Info"
secret key: See "Secure Info"

arn:aws:s3:::mr-hostedmail


test it
s3cmd  --access_key=<access key> --secret_key=<secret key>   put ./add.txt  s3://mr-hostedmail



building

Install rust, via rust-up:

curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh


Update rust: 
  # rustup



cd /usr/local/mroute/install/src/mail-server # !cargo
cargo build --manifest-path=crates/main/Cargo.toml --no-default-features --features s3,mysql,foundationdb,elastic,redis --release


elastic 
	user: stalwart
	password: See "Secure Note"
	
	index name: stalwart_email
	
	
	
	
### mysql stored procedures:

```

DELIMITER //
     DROP PROCEDURE StalwartGetPrimaryEmailAddress;
     CREATE PROCEDURE StalwartGetPrimaryEmailAddress(IN full_email_address VARCHAR(255))
     BEGIN
       DECLARE local_part VARCHAR(255);
       DECLARE domain_part VARCHAR(255);

       SET local_part = SUBSTRING_INDEX(full_email_address, '@', 1);
       SET domain_part = SUBSTRING(full_email_address, INSTR(full_email_address, '@') + 1);


       SELECT DISTINCT CONCAT(e.localpart, '@', d.name)
       FROM mroute_admin.amavisd_localpartalias la
       LEFT JOIN mroute_admin.amavisd_domainalias da ON la.domain_id = da.domain_id
       LEFT JOIN mroute_admin.amavisd_domain d ON da.domain_id = d.id
       LEFT JOIN mroute_admin.amavisd_emailaccount e ON la.email_account_id = e.id
       WHERE la.localpart = local_part
         AND (la.external_domain = domain_part OR da.name = domain_part);
   END;
//
DELIMITER ;

DELIMITER //
   DROP PROCEDURE StalwartGetUserInfo;
   CREATE PROCEDURE StalwartGetUserInfo(IN full_email_address VARCHAR(255))
   BEGIN
       DECLARE local_part VARCHAR(255);
       DECLARE domain_part VARCHAR(255);

       -- Split the full email address into local part and domain part
       SET local_part = SUBSTRING_INDEX(full_email_address, '@', 1);
       SET domain_part = SUBSTRING(full_email_address, INSTR(full_email_address, '@') + 1);


       -- Execute the query using the local part and domain part
           SELECT
           NULL AS quota,
           'individual' AS type,
           'full name' AS description,
           CONCAT(e.localpart, '@', d.name) AS name,
           IF(
              LEFT(u.password, 12) = 'sha256_crypt',
              SUBSTRING(u.password FROM 13),
              u.password
           ) AS secret
       FROM
           mroute_admin.auth_user u
       LEFT JOIN mroute_admin.amavisd_emailaccount e 
           ON u.id=e.user_id
       LEFT JOIN mroute_admin.amavisd_localpartalias la 
           ON e.id=la.email_account_id
       LEFT JOIN mroute_admin.amavisd_domainalias da 
           ON la.domain_id=da.domain_id
       LEFT JOIN mroute_admin.amavisd_domain d 
           ON la.domain_id=d.id
       LEFT JOIN mroute_admin.amavisd_policy policy_domain 
           ON d.policy_id = policy_domain.id
       WHERE
           (
           la.localpart=local_part 
           AND (da.name = domain_part OR la.external_domain = domain_part) 
           AND policy_domain.hosted_email_enabled IN ('y', 'Y')
           );    
   END;

//
DELIMITER ;


DELIMITER //

CREATE PROCEDURE StalwartGetNULL(IN full_email_address VARCHAR(255))
BEGIN
SELECT NULL;
END;

//
DELIMITER ;




DELIMITER //
DROP PROCEDURE StalwartGetAlLEmails;
CREATE PROCEDURE StalwartGetAlLEmails(IN full_email_address VARCHAR(255))
BEGIN
    DECLARE local_part VARCHAR(255);
    DECLARE domain_part VARCHAR(255);
    DECLARE primary_email VARCHAR(255);

    -- Extract the local and domain parts from the input email
    SET local_part = SUBSTRING_INDEX(full_email_address, '@', 1);
    SET domain_part = SUBSTRING(full_email_address, INSTR(full_email_address, '@') + 1);


    -- Find the primary email address directly using joins
    SELECT DISTINCT CONCAT(e.localpart, '@', d.name) INTO primary_email
    FROM mroute_admin.amavisd_localpartalias la
    LEFT JOIN mroute_admin.amavisd_domainalias da ON la.domain_id = da.domain_id
    LEFT JOIN mroute_admin.amavisd_domain d ON da.domain_id = d.id
    LEFT JOIN mroute_admin.amavisd_emailaccount e ON la.email_account_id = e.id
    WHERE la.localpart = local_part
      AND (la.external_domain = domain_part OR da.name = domain_part)
    LIMIT 1;

    -- Select email addresses and prioritize the primary email
    SELECT DISTINCT 
        CONCAT(a, '@', b) AS email 
    FROM 
        (SELECT DISTINCT la.localpart AS a 
         FROM mroute_admin.amavisd_localpartalias la 
         LEFT JOIN mroute_admin.amavisd_localpartalias la2 
           ON la.email_account_id = la2.email_account_id 
         WHERE la2.localpart = local_part 
           AND la2.domain_id IN 
               (SELECT domain_id FROM mroute_admin.amavisd_domainalias WHERE name = domain_part)
        ) AS t1 
    CROSS JOIN 
        (SELECT DISTINCT da.name AS b 
         FROM mroute_admin.amavisd_domainalias da 
         LEFT JOIN mroute_admin.amavisd_domainalias da2 
           ON da.domain_id = da2.domain_id 
         WHERE da2.name = domain_part
        ) AS t2 

    UNION 

    SELECT 
        CONCAT(lae.localpart, '@', lae.external_domain) 
    FROM mroute_admin.amavisd_localpartalias lae 
    WHERE lae.external_domain IS NOT NULL 
      AND lae.localpart = local_part 
      AND lae.domain_id IN 
          (SELECT domain_id FROM mroute_admin.amavisd_domainalias WHERE name = domain_part)  

    ORDER BY 
        (CASE 
            WHEN email = primary_email THEN 0 
            ELSE 1 
        END), 
        email;
END;
//
DELIMITER ;





```


# Building stalwart

```

   
rustup override unset

cargo build --manifest-path=crates/main/Cargo.toml --features s3,mysql,foundationdb,elastic,redis,rocks,enterprise --release

cargo build -p stalwart-cli --release


built binary in target/release - stalwart, stalwart-cli


stop all stalwart instances in cluster.

Then 
```
