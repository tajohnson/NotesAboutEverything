Find domains where the MX record is NOT mailroute.net:
   select domain.domain,domain.created,SUBSTRING(changed_to,1,40) from mx_history,domain where mx_history.domain_id=domain.id AND mx_history.changed_to  NOT LIKE '%mailroute%' AND domain.created < '2010-09-28' ORDER by domain.created;


Copy those domains into the domain_deleted table:
	insert into domain_deleted  (select * from domain where id in (select domain.id from mx_history,domain where mx_history.domain_id=domain.id AND mx_history.changed_to  NOT LIKE '%mailroute%' AND domain.created < '2010-09-28' ORDER by domain.created));


Delete them from the domain table
	delete from domain where id in   (select domain.id from mx_history,domain where mx_history.domain_id=domain.id AND mx_history.changed_to  NOT LIKE '%mailroute%' AND domain.created < '2010-09-28' ORDER by domain.created);


Remove the domains that are in domain_deleted from domain:
	 delete from domain where id in (SELECT id from domain_deleted);
