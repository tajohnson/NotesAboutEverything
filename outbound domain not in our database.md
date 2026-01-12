select     time_iso,m.email,d.name, c.name, r.name  from msgs     left join maddr as m       on msgs.sid=m.id and msgs.partition_tag=m.partition_tag    left join mroute_admin.amavisd_outboundserver as os      on client_addr = os.server    left join mroute_admin.amavisd_domain as d      on os.domain_id=d.id    left join mroute_admin.amavisd_customer as c      on d.customer_id=c.id    left join mroute_admin.amavisd_reseller as r      on c.reseller_id=r.id    where       m.email != ''       and       originating = 'Y'        and        client_addr not like '199.89.%'        and not exists (select id from mroute_admin.amavisd_domainalias where name = right(m.email, length(m.email)-INSTR(m.email, '@')));




cat ./*/amavis_outbound-2021* | grep -v "mr . .  "  |  awk '{print $8, $(NF-1), $1, $2}' >> /tmp/senders