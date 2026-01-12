
rm -rf /var/venv/dev /var/venv/dev_old_node /var/www/dev /var/www/dev-frontend


# for testing
Dump database schema and data (leave out the  unneeded, huge tables):

```
mysqldump -h mysql-read.mailroute.net  -u admin -p mroute_admin > /tmp/mroute_admin_schema.sql

mysqldump -h mysql-read.mailroute.net  -u admin -p  --ignore-table=mroute_admin.amavisd_o365syncresult --ignore-table=mroute_admin.amavisd_ldapsyncresult --ignore-table=mroute_admin.amavisd_gsuitesyncresult --ignore-table=mroute_admin.reversion_version --ignore-table=mroute_admin.reversion_revision --ignore-table=mroute_admin.audittrail_historyrecord --ignore-table=mroute_admin.temporary_usercounts --ignore-table=mroute_admin.temporary_mxtrends --ignore-table=mroute_admin.django_celery_beat_crontabschedule --ignore-table=mroute_admin.django_celery_beat_periodictask mroute_admin > /tmp/mroute_admin_data.sql

```

- [ ] 