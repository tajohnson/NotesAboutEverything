

pt-archiver --source h=db-amavisd-rw.mailroute.net,D=policyd,t=triplet,u=admin,p=<pw> --no-check-charset --primary-key-only --progress 25000   --purge --bulk-delete --limit=25000 --where "_datelast < unix_timestamp(NOW())-2592000" 


pt-archiver --source h=db-amavisd-rw.mailroute.net,D=policyd_outbound,t=throttle_from_instance,u=admin,p=<pw> --no-check-charset --primary-key-only --progress 5000   --purge --bulk-delete --limit=5000 --where "_expire < unix_timestamp(NOW())-3600" 

pt-archiver --source h=db-amavisd-rw.mailroute.net,D=policyd_outbound,t=throttle,u=admin,p=<pw> --no-check-charset --primary-key-only --progress 25000   --purge --bulk-delete --limit=25000 --where "_date < unix_timestamp(NOW())-2592000 and _rcpt_max < 2001" 

pt-archiver --source h=db-amavisd-rw.mailroute.netdb-amavisd-rw.mailroute.net,D=policyd_smtpauth,t=throttle_from_instance,u=admin,p=<pw> --no-check-charset --primary-key-only --progress 25000   --purge --bulk-delete --limit=25000 --where "_expire < unix_timestamp(NOW())-3600" 

pt-archiver --source h=db-amavisd-rw.mailroute.net,D=policyd_smtpauth,t=throttle,u=admin,p=<pw> --no-check-charset --primary-key-only --progress 25000   --purge --bulk-delete --limit=25000 --where "_date < unix_timestamp(NOW())-2592000 and _rcpt_max < 2001" 


pt-archiver --source h=db-amavisd-rw.mailroute.net,D=mroute_admin,t=django_session,u=admin,p=<pw> --no-check-charset --primary-key-only --progress 10000   --purge --bulk-delete --limit=10000 --where "expire_date < NOW()" 



pt-archiver --source h=db-amavisd-rw.mailroute.net,D=mroute_admin,t=reversion_revision,u=admin,p=<pw> --skip-foreign-key-checks --no-check-charset --primary-key-only --progress 1000   --purge --bulk-delete --limit=1000 --where "date_created < '2021-01-01'" 

pt-archiver --source h=db-amavisd-rw.mailroute.net,D=mroute_admin,t=reversion_version,u=admin,p=<pw> --skip-foreign-key-checks --no-check-charset --primary-key-only --progress 25000   --purge --bulk-delete --limit=25000 --where "revision_id not in (SELECT id from reversion_revision where id=reversion_version.revision_id)" 


pt-archiver --source h=db-amavisd-rw.mailroute.net,D=mroute_admin,t=audittrail_historyrecord,u=admin,p=<pw> --skip-foreign-key-checks --no-check-charset --primary-key-only --progress 1000   --purge --bulk-delete --limit=1000 --where "date_created < '2021-01-01'" 


pt-archiver --source h=db-amavisd-rw.mailroute.net,D=mroute_admin,t=amavisd_gsuitesyncresult,u=admin,p=<pw> --no-check-charset --primary-key-only --progress 500   --purge --bulk-delete --limit=500 --where "created_at < '2024-01-01'" 

pt-archiver --source h=db-amavisd-rw.mailroute.net,D=mroute_admin,t=amavisd_ldapsyncresult,u=admin,p=<pw> --no-check-charset --primary-key-only --progress 500   --purge --bulk-delete --limit=500 --where "created_at < '2024-01-01'" 

pt-archiver --source h=db-amavisd-rw.mailroute.net,D=mroute_admin,t=amavisd_o365syncresult,u=admin,p=<pw> --no-check-charset --primary-key-only --progress 500   --purge --bulk-delete --limit=500 --where "created_at < '2024-01-01'" 




# TEST THIS - NOT SURE YET IF IT"S OKAY TO DO THIS
#pt-archiver --source h=db-amavisd-rw.mailroute.net,D=mroute_admin,t=amavisd_wblist,u=admin,p=<pw> --no-check-charset --primary-key-only --progress 25000   --purge --bulk-delete --limit=25000 --where "rid not in (SELECT uuid from amavisd_domain UNION SELECT uuid from amavisd_emailaccount)" 

pt-archiver --source h=db-amavisd-rw.mailroute.net,D=mroute_admin,t=amavisd_mailaddr,u=admin,p=<pw> --no-check-charset --primary-key-only --progress 25000   --purge --bulk-delete --limit=25000 --where "id not in (SELECT sid from amavisd_wblist)" 



use policyd_outbound;
optimize table throttle;
optimize table throttle_from_instance;

use policyd;
optimize table triplet;


use mroute_admin

optimize table django_session;
optimize table amavisd_ldapsyncresult;
optimize table amavisd_ldapsyncresultsummary;
optimize table amavisd_o365syncresult;
optimize table amavisd_gsuitesyncresult;
optimize table reversion_revision;
optimize table reversion_version;
optimize table audittrail_historyrecord;
optimize table amavisd_mailaddr;











use mroute_admin;
select count(*) from django_session where expire_date < NOW();
delete from django_session where expire_date < NOW();

use policyd_outbound;
select count(*) from throttle_from_instance where _expire < unix_timestamp(NOW())-3600;
delete from throttle_from_instance where _expire < unix_timestamp(NOW())-3600;
select count(*) from throttle where _date < unix_timestamp(NOW())-2592000 and _rcpt_max < 2001;
delete from throttle where _date < unix_timestamp(NOW())-2592000 and _rcpt_max < 2001;

use policyd_smtpauth;
select count(*) from throttle_from_instance where _expire < unix_timestamp(NOW())-3600;
delete from throttle_from_instance where _expire < unix_timestamp(NOW())-3600;
select count(*) from throttle where _date < unix_timestamp(NOW())-2592000 and _rcpt_max < 2001;
delete from throttle where _date < unix_timestamp(NOW())-2592000 and _rcpt_max < 2001;
