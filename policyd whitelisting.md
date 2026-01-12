insert ignore into whitelist (select os.server,concat('# customer - ', d.name),0 from mroute_admin.amavisd_outboundserver as os left join mroute_admin.amavisd_domain as d on os.domain_id = d.id);


