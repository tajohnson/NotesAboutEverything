
pt-archiver --source h=localhost,D=policyd_outbound,t=throttle_from_instance,u=root,p=man0uver --no-check-charset --primary-key-only --progress 25000   --purge --bulk-delete --limit=25000 --where "_expire < unix_timestamp(NOW())-3600"
pt-archiver --source h=localhost,D=policyd_outbound,t=throttle,u=root,p=man0uver --no-check-charset --primary-key-only --progress 25000   --purge --bulk-delete --limit=25000 --where "_date < unix_timestamp(NOW())-2592000 and _rcpt_max < 2001"

pt-archiver --source h=localhost,D=policyd,t=triplet,u=root,p=man0uver --no-check-charset --primary-key-only --progress 25000   --purge --bulk-delete --limit=25000 --where "_datelast < unix_timestamp(NOW())-2592000" &
pt-archiver --source h=localhost,D=policyd,t=triplet,u=root,p=man0uver --no-check-charset --primary-key-only --progress 25000   --purge --bulk-delete --limit=25000 --where "_datenew < unix_timestamp(NOW())-172800 AND _count = 0" &


pt-archiver --source h=localhost,D=mroute_admin,t=django_session,u=root,p=man0uver --no-check-charset --primary-key-only --progress 25000   --purge --bulk-delete --limit=25000 --where "expire_date < NOW()"


SET foreign_key_checks = 0;

delete from django_session where expire_date < NOW();

select count(*) from amavisd_ldapsyncresult WHERE created_at < NOW() - INTERVAL 14 DAY;
SET foreign_key_checks = 0; DELETE FROM amavisd_ldapsyncresult WHERE created_at < NOW() - INTERVAL 14 DAY limit 1000;SET foreign_key_checks = 1;

select count(*) from amavisd_gsuitesyncresult WHERE created_at < NOW() - INTERVAL 14 DAY;
SET foreign_key_checks = 0;delete from amavisd_gsuitesyncresult WHERE created_at < NOW() - INTERVAL 14 DAY limit 1000 ;SET foreign_key_checks = 1;

select count(*) from amavisd_o365syncresult WHERE created_at < NOW() - INTERVAL 7 DAY;
SET foreign_key_checks = 0;delete from amavisd_o365syncresult WHERE created_at < NOW() - INTERVAL 7 DAY limit 10000 ;SET foreign_key_checks = 1;




optimize table reversion_revision;
optimize table reversion_version;







(ssh root@199.89.0.45  mysqldump -uroot -p --no-data --routines --triggers --databases mroute_admin  mroute_logquar policyd policyd_outbound  admin_dev_v2 admin_dev2 roundcube) > /var/lib/mysql/schema.sql

 (ssh root@199.89.0.45  mysqldump -u root --password=man0uver  --set-gtid-purged=AUTO --single-transaction --skip-add-locks --databases mroute_admin  mroute_logquar policyd policyd_outbound  admin_dev_v2 admin_dev2 roundcube) | mysql -u root --password=man0uver


