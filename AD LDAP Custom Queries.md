ldapsearch  -Hldaps://24.213.216.233:636  -D mailroute@yatesarc.local -w 836dMJ3S1g8Tc5R6 -b 'dc=yatesarc,dc=local' 



Skip ResourceType:Room, ResourceType:Equipment, etc....

(& (mailnickname=*)(!(msExchResourceMetaData=ResourceType:*))(!(mailnickname=discoverysearchmailbox*))(!(mailnickname=federatedemail*))(!(mailnickname=systemmailbox*)) (| (objectClass=publicFolder) (&(objectCategory=person) (objectClass=user) (!(homeMDB=*)) (!(msExchHomeServerName=*))) (&(objectCategory=person) (objectClass=user) (|(homeMDB=*) (msExchHomeServerName=*))) (objectCategory=person) (objectCategory=group) (objectClass=msExchDynamicDistributionList) ) (!(objectClass=contact)) )


Domino setting?

(&(|(objectclass=dominoPerson)(objectclass=dominoGroup)(objectclass=dominoServerMailInDatabase))(mail=*))

ldapsearch  -Hldap://mail.dor.org:389  -D "CN=test zz,OU=Pastoral Center,O=Diocese of Rochester" -w rdLOrTdFw6Av '(&(|(objectclass=dominoPerson)(objectclass=dominoGroup)(objectclass=dominoServerMailInDatabase))(maildomain=DOR)(mail=*)(!(mail=*doradmin.net)))'



ldapsearch  -Hldap://mail.papco.com:686  -D "CN=test zz,OU=Pastoral Center,O=Diocese of Rochester" -w password -b 'o=Diocese of Rochester'

ldapsearch  -Hldaps://mail.papco.com:686  -D "mailroute-ldap-read@papco.com" -w 2ZJ7pHAk -b 'dc=papco,dc=com' 


ldapsearch  -v -Hldap://mail.dor.org:389  -D "CN=test zz,OU=Pastoral Center,O=Diocese of Rochester" -w password -b 'o=Diocese of Rochester' mail

ldapsearch  -v -Hldap://mail.lcn.kmmfg.com:389  -D pkramer -w 'eye!cale' -b '' '(|(&(maildomain=kmmfg)(mail=*lcn.kmmfg.com)(!(mail=*KMMFG.lcn.kmmfg.com))(objectclass=dominoPerson))(&(objectClass=groupofNames) (mail=*lcn.kmmfg.com)))' mail cn



ldapsearch  -v -Hldap://mail.denverplastics.com:389  -D andyk@denverplastics.com -w ak0322 -b 'dc=denverplastics,dc=com'

               id: 1028
          enabled: 0
          dc_list: mail.denverplastics.com
             port: 389
             user: andyk@denverplastics.com
         password: ak0322
          base_dn: dc=denverplastics,dc=com
    search_string: 
       sync_count: 1
        domain_id: 489
          task_id: 86769
           parser: 
 recipient_emails: 
notify_recipients: 0
          use_ssl: 0
        mail_attr: mail
    aliases_attrs: proxyAddresses

ldapsearch  -v -Hldap://184.148.16.149:389  -D mail@zenetec.com -w Mspress#1 -b 'dc=zenetec,dc=com' mail proxyAddresses



ldapsearch  -v -Hldaps://connect.netprospex.com:636  -D Admin -w 'Np94m3$' -b 'fn=ContactRoot'




ldapsearch  -v -Hldap://221.120.193.205:389  -D muhammad.imran@ffbl.com -w UmerBinImran2010 -b 'O=ffbl' mail uid
ldapsearch  -v -Hldap://221.120.193.205:389  -D muhammad.imran@ffbl.com -w UmerBinImran2010 



ldapsearch  -v -Hldap://24.246.118.199:389  -D 'cn=admin,o=btsd' -w dnetit1999 


ldapsearch  -v -Hldap://24.246.118.199:389  -D 'cn=admin,o=btsd' -w dnetit1999  -b o=btsd '(objectClass=groupWiseExternalEntity)' mail | grep mail

ldapsearch  -v -Hldap://24.246.118.199:389  -D 'cn=admin,o=btsd' -w dnetit1999  -b o=btsd '(|(objectclass=person)(objectclass=inetOrgPerson))'



ldapsearch  -v -Hldaps://mail.ssrights.com:636  -D 'cn=mailroute,ou=services,o=ww' -w pesach14  -b o=ww 




ldapsearch  -v -Hldap://216.56.144.139:389  -D mailroute -w Wednesday14 

ldapsearch  -v -Hldap://217.36.255.243:389  -D afn@dolphinappliance.co.uk -w Zapiekanka14101410 -b 'dc=da,dc=local' 



ldapsearch  -v -Hldaps://71.6.117.185:636  -D 'CN=AD Bind Service Account,OU=SBSUsers,OU=Users,OU=MyBusiness,DC=internal,DC=financialknowledge,DC=com' -w "$fkADSVCphones798"  -b OU=MyBusiness,DC=internal,DC=financialknowledge,DC=com 



ldapsearch  -Hldap://wars1.bowencenter.org:389  -D "CN=Mail Route,CN=Users,DC=bowen,DC=lcl" -w "s3xJHyHveuRJ2tK&HwjZv*GkvcfVodyM" -b 'DC=bowen,DC=lcl'


ldapsearch -Hldap://autodiscover.ballventures.com:389 -D info@ballventures.com -w Bela1212




ldapsearch  -v -Hldap://209.150.64.139:389  -D 'Mailrouteldap@cppcorp.com' -w "M@1lr0ute"  -b dc=cppcorp,dc=com



ldapsearch  -v -Hldap://208.124.240.26:389  -D 'dirsync' -w Summer2014 -b DC=swisscanadian,DC=local



ldapsearch  -v -Hldap://98.172.0.126:389  -D 'administrator@outdoorcap.com' -w OutD0orC@p -b dc=outdoorcap,dc=com

ldapsearch  -v -Hldap://mail.lexidc.com  -D 'cn=lidcldap,dc=lexidc,dc=local' -w C3ntr1c1ty@1 -b dc=lexidc,dc=local




ldapsearch  -v -Hldap://128.177.70.120:390  -D 'CN=mailroute,CN=Users,DC=mysdi,DC=local' -w MR5rox2! -b DC=mysdi,DC=local

ldapsearch  -v -Hldap://208.95.194.155:389  -D 'cn=ts8089,ou=it,ou=crunchhq,dc=crunchcorp,dc=com' -w F33lzG00d! -b dc=crunchcorp,dc=com


ldapsearch  -v -Hldap://208.185.33.37:389  -D 'CN=mailroute,CN=Users,DC=mysditest,DC=local' -w 'Far2BawaY77' -b DC=mysditest,DC=local

ldapsearch  -v -Hldap://128.177.70.120:389  -D 'CN=mailroute,CN=Users,DC=mysdi,DC=local' -w 'MR5rox2!' -b DC=mysdi,DC=local



ldapsearch  -v -Hldaps://66.42.247.200:389  -D 'mailroute' -w 'B#Z!9418@' -b cn=users,dc=Xserve01,dc=beechmont,dc=private



ldapsearch  -v -Hldap://207.145.117.10:636  -D 'mailroute@brentwoodit.local' -w 'pass:word:2014' -b dc=brit-sbs-01-p,dc=brentwoodit,dc=com


ldapsearch  -v -Hldap://207.145.117.10:636  -D 'mailroute@brentwoodit.local' -w 'pass:word:2014' -b dc=brit-sbs-01-p,dc=brentwoodit,dc=local


ldapsearch  -v -Hldaps://65.183.4.110:389  -D 'steve.whyte' -w 'Heather1' -b  cn=hcsjm.com

ldapsearch  -v -Hldap://65.183.4.110:389  -D 'steve.whyte' -w 'Heather1' -b  cn=kwljm.com




ldapsearch  -v -Hldap://jr39ujueezemy5lcxxdtbuqgy2m2jtup.kippnyc.org:389  -D 'mailroute@kippnyc.kipp.org' -w 'h3b8ubruP5ASpaYA' -b  dc=kippnyc,dc=kipp,dc=org '(& 	(mailnickname=*)	(!(mailnickname=discoverysearchmailbox*))	(!(mailnickname=federatedemail*))	(!(mailnickname=systemmailbox*))	(!(mail=*.local))	(|		(objectClass=publicFolder)		(&(objectCategory=person)(objectClass=user)(!(homeMDB=*))(!(msExchHomeServerName=*)))		(&(objectCategory=person)(objectClass=user)(|(homeMDB=*)(msExchHomeServerName=*)))		(objectCategory=person)		(objectCategory=group)		(objectClass=msExchDynamicDistributionList)	)	(&(!(msExchRequireAuthToSendTo=TRUE))) 	(!(objectClass=contact)))'




ldapsearch  -v -Hldap://208.95.194.155:389  -D 'cn=ts8089,ou=it,ou=crunchhq,dc=crunchcorp,dc=com' -w 'F33lzG00d!' -b  ou=crunchhq,dc=crunchcorp,dc=com
ldapsearch  -v -Hldap://208.95.194.155:389  -D 'cn=ts8089,ou=it,ou=crunchhq,dc=crunchcorp,dc=com' -w 'F33lzG00d!' -b  dc=crunchcorp,dc=com



namingContexts: DC=crunchcorp,DC=com
namingContexts: CN=Configuration,DC=crunchcorp,DC=com
namingContexts: CN=Schema,CN=Configuration,DC=crunchcorp,DC=com
namingContexts: DC=DomainDnsZones,DC=crunchcorp,DC=com
namingContexts: DC=ForestDnsZones,DC=crunchcorp,DC=com



msExchDynamicDistributionList



ldapsearch  -v -Hldaps://corp-exch2.colorlabcosmetics.com:636  -D 'administrator@colorlabcosmetics.com' -w 'CoLorlAb-IT' -b  DC=ColorlabCosmetics,DC=com


ldapsearch  -v -Hldap://mail.sgrl.org:389  -D 'administrator@sgrl10.sqrl.org' -w 'sms4714x!@#' -b  DC=sgrl10,DC=sgrl,DC=org





ldapsearch  -v -Hldaps://76.11.51.14:636  -D 'CN=App MailRoute,OU=Service Accounts,DC=lan,DC=wetaskiwin,DC=ca' -w 'YdAcuSTxoSCcLv8!@#' -b  DC=lan,DC=wetaskiwin,DC=ca



ldapsearch  -v -Hldap://152.180.6.225  -D swilliams@imls.gov -w 'Th3*Br33z3!' -b  CN=IMLS-ADC1,OU=Domain Controllers,DC=IMLS,DC=gov



ldapsearch  -v -Hldap://mail.ctobacco.com  -D cpi\brian -w '711hbl&BTR' -b  'dc=cpi'




ldapsearch  -v -Hldaps://206.19.205.29:389  -D mailroute@childrenshomeandaid.org -w 'Mapscha!' -b  OU=People,DC=childrenshomeandaid,DC=org




ldapsearch  -v -Hldaps://ldap.smartdraw.com:636  -D test@smartdraw.com -w '62Middleton' -b  ou=SDOffice,dc=smartdraw,dc=com



ldapsearch  -v -Hldaps://96.35.143.155:636  -D mroute@atozpediatrics.com -w 'Bubblewall1230' -b  dc=ad,dc=atozpediatrics,dc=com


ldapsearch  -v -Hldap://power-soft.net:389  -D administrator@power-soft.net -w 'freelander' -b  cn=Users,dc=power-soft,dc=net






# ldapsearch  -v -Hldap://server.principaledge.com.au:389  -D mr@principaledge.lan -w 'Tr33$soil' -b  dc=principaledge,dc=lan '(& (mailnickname=*)(!(mailnickname=discoverysearchmailbox*))(!(mailnickname=federatedemail*))(!(mailnickname=systemmailbox*))(!(mail=*.local))(|(objectClass=publicFolder)(&(objectCategory=person)(objectClass=user)(!(homeMDB=*))(!(msExchHomeServerName=*)))(&(objectCategory=person)(objectClass=user)(|(homeMDB=*)(msExchHomeServerName=*)))(objectCategory=person)(objectCategory=group)(objectClass=msExchDynamicDistributionList))(&(!(msExchRequireAuthToSendTo=TRUE))) (!(objectClass=contact)))' mail proxyAddresses



ldapsearch  -v -Hldaps://ldap.exch022.serverdata.net:636  -D 'jnichols@unboundtechnology.com' -w 'kl0d-h0pr' -b  'OU=UnboundExchange,OU=Hosting,DC=exch022,DC=domain,DC=local'


ldapsearch  -v -Hldaps://50.204.8.224:389  -D 'mailroute@kipphou.kipp.org' -w 'K!PPhou!@#$' -b  'dc=kipphou,dc=kipp,dc=org'




ldapsearch  -v -Hldap://206.19.205.29:389  -D 'mailroute@childrenshomeandaid.org' -w 'Mapscha!' -b  'OU=People,DC=childrenshomeandaid,DC=org'

ldapsearch  -v -Hldap://206.19.205.29:389  -D 'mailroute@childrenshomeandaid.org' -w 'Mapscha!' -b  'OU=NonUserActiveEmails,OU=NonUser,OU=People-Inactive and NonUser,DC=childrenshomeandaid,DC=org'



ldapsearch  -v -Hldap://mail.1128.com:389  -D 'mailrouteldap@centurionbet.local' -w 'pa6DR6qvP0n9ecs94IKD' -b  'dc=CENTURIONBET,dc=local'



ldapsearch  -v -Hldaps://162.211.3.130:636  -D 'spamadmin@cablecomllc.local' -w 'luqFMDq!fskKI' -b  'dc=cablecomllc,dc=local'




ldapsearch  -v -Hldaps://38.104.208.242:636  -D 'infgrp\mailroute' -w 'CRMSONuser70' -b  'OU=HRBrain,OU=INFGRP,DC=INFGRP,DC=LOCALl' mail proxyAddresses

ldapsearch  -v -Hldaps://24.213.216.233:636  -D 'mailroute@yatesarc.local' -w '836dMJ3S1g8Tc5R6' -b  'dc=yatesarc,dc=local' '(& (mailnickname=*)(!(msExchResourceMetaData=ResourceType:*))(!(mailnickname=discoverysearchmailbox*))(!(mailnickname=testuser*))(!(mailnickname=waiverdatabase*))(!(mailnickname=smartboard*))(!(mailnickname=searchresults*))(!(mailnickname=journalmailbox*))(!(mailnickname=helpdesk*))(!(mailnickname=happinesshotline*))(!(mailnickname=fax*))(!(mailnickname=aimcommittee*))(!(mailnickname=allstaff*))(!(mailnickname=federatedemail*))(!(mailnickname=systemmailbox*)) (| (objectClass=publicFolder) (&(objectCategory=person) (objectClass=user) (!(homeMDB=*)) (!(msExchHomeServerName=*))) (&(objectCategory=person) (objectClass=user) (|(homeMDB=*) (msExchHomeServerName=*))) (objectCategory=person) (objectCategory=group) (objectClass=msExchDynamicDistributionList) ) (!(objectClass=contact)) )' mail proxyAddresses







ldapsearch  -v -Hldap://208.46.172.167:389  -D 'mailroute@hydraulx.com' -w '5Dig12zvy&8142' -b  'dc=dc,dc=hydraulx,dc=com' mail proxyAddresses


ldapsearch  -v -Hldap://ldap.actforchildren.org:389  -D 'ldapagent@dcac.internal' -w 'd7bhRju3t4tyrZ' -b  'ou=iafc,dc=dcac,dc=internal' mail proxyAddresses

ldapsearch  -v -Hldap://ldap.actforchildren.org:389  -D 'ldapagent@dcac.internal' -w 'd7bhRju3t4tyrZ' -b  'ou=iafc,dc=dcac,dc=internal' '(& (mailnickname=*)(!(msExchResourceMetaData=ResourceType:*))(!(mailnickname=discoverysearchmailbox*))(!(mailnickname=testuser*))(!(mailnickname=waiverdatabase*))(!(mailnickname=smartboard*))(!(mailnickname=searchresults*))(!(mailnickname=journalmailbox*))(!(mailnickname=helpdesk*))(!(mailnickname=happinesshotline*))(!(mailnickname=fax*))(!(mailnickname=aimcommittee*))(!(mailnickname=allstaff*))(!(mailnickname=federatedemail*))(!(mailnickname=systemmailbox*)) (| (objectClass=publicFolder) (&(objectCategory=person) (objectClass=user) (!(homeMDB=*)) (!(msExchHomeServerName=*))) (&(objectCategory=person) (objectClass=user) (|(homeMDB=*) (msExchHomeServerName=*))) (objectCategory=person) (objectCategory=group) (objectClass=msExchDynamicDistributionList) ) (!(objectClass=contact))(!(userAccountControl:1.2.840.113556.1.4.803:=2)) )' mail proxyAddresses





ldapsearch  -v -Hldap://63.149.239.199:389  -D 'dc=Administrator,dc=int,dc=hdlx,dc=us' -w 'd7bhRju3t4tyrZ' -b  'dc=int,dc=hdlx,dc=us' mail proxyAddresses
ldapsearch  -v -Hldap://63.149.239.199:389  -D 'dc=Administrator,dc=int,dc=hdlx,dc=us' -w 'd7bhRju3t4tyrZ' -b  'dc=access,dc=int,dc=hdlx,dc=us' mail proxyAddresses



ldapsearch  -v -Hldap://38.140.48.188:389  -D 'ldap@averna-us.com' -w 'Avpass72!' -b  'cn=MailRoute,cn=Users,dc=averna-us,dc=com' mail proxyAddresses


ldapsearch  -v -Hldaps://sslvpn.fcbmd.com:64636  -D 'CN=mailroute-query,OU=Local Users,DC=fcbmd-email,DC=com' -w 'wiDgERvJpojQVm(XNrduB6Ch' -b  'DC=fcbmd-email,DC=com' '(& (mail=*@fcbmd.com)(!(distinguishedName=*OU=_Disabled,DC=fcbmd-email,DC=com)))' mail proxyAddresses

ldapsearch  -v -Hldaps://24.213.216.233:636  -D 'mailroute@yatesarc.local' -w '836dMJ3S1g8Tc5R6' -b  'dc=yatesarc,dc=local' mail proxyAddresses



ldapsearch  -v -Hldaps://216.8.196.131:636  -D 'mailroute_ldap@stc.net' -w 'dez-waf-tresp-tat-triy-fust-mib-skuct' -b  'CN=Users,dc=stc,dc=net' mail proxyAddresses objectclass
ldapsearch  -v -Hldaps://216.8.196.131:636  -D 'mailroute_ldap@stc.net' -w 'dez-waf-tresp-tat-triy-fust-mib-skuct' -b  'OU=Groups,dc=stc,dc=net=net' mail proxyAddresses objectclass
ldapsearch  -v -Hldaps://216.8.196.131:636  -D 'mailroute_ldap@stc.net' -w 'dez-waf-tresp-tat-triy-fust-mib-skuct' -b  'OU=User Accounts,dc=stc,dc=net' mail proxyAddresses objectclass
ldapsearch  -v -Hldaps://216.8.196.131:636  -D 'mailroute_ldap@stc.net' -w 'dez-waf-tresp-tat-triy-fust-mib-skuct' -b  'OU=Distribution Lists,dc=stc,dc=net' mail proxyAddresses objectclass





ldapsearch  -v -Hldap://172.254.75.149:389  -D 'wschellhaas@crunchcorp.com' -w 'Sms0813Wss0126Mes0120' -b  'dc=crunchcorp,dc=com' mail proxyAddresses objectclass

ldapsearch  -v -Hldap://172.254.75.149:389  -D 'wschellhaas@crunchcorp.com' -w 'Sms0813Wss0126Mes0120' -b  'ou=crunchhq,dc=crunchcorp,dc=com' mail proxyAddresses objectclass


d

ldapsearch  -v -Hldap://mx.bl3ndlabs.com:389  -D 'uid=zmpostfix,cn=appaccts,cn=zimbra' -w 'XLRKydMEOe' -b  'dc=bl3ndlabs,dc=com' zimbraMailDeliveryAddress zimbraMailAlias zimbraDistributionList objectclass




ldapsearch  -v -Hldaps://216.8.196.131:636  -D 'mailroute_ldap@stc.net' -w 'dez-waf-tresp-tat-triy-fust-mib-skuct' -b  'OU=User Accounts,dc=stc,dc=net' mail proxyAddresses objectclass
ldapsearch  -v -Hldaps://216.8.196.131:636  -D 'mailroute_ldap@stc.net' -w 'dez-waf-tresp-tat-triy-fust-mib-skuct' -b  'OU=Distribution Lists,dc=stc,dc=net' mail proxyAddresses objectclass
ldapsearch  -v -Hldaps://216.8.196.131:636  -D 'mailroute_ldap@stc.net' -w 'dez-waf-tresp-tat-triy-fust-mib-skuct' -b  'OU=Security Groups,dc=stc,dc=net' mail proxyAddresses objectclass
ldapsearch  -v -Hldaps://216.8.196.131:636  -D 'mailroute_ldap@stc.net' -w 'dez-waf-tresp-tat-triy-fust-mib-skuct' -b  'CN=Users,dc=stc,dc=net' mail proxyAddresses objectclass



ldapsearch  -v -Hldap://209.216.46.112:389  -D 'tony@buybypin.com' -w '7813@2020' -b  ' dc=buybypin,dc=com' 




ldapsearch  -v -Hldap://209.216.46.112:389  -D 'root@daor.com' -w '7813savuth$' -b  ' cn=root,o=daor.com' 




ldapsearch  -v -Hldap://96.32.123.38:389  -D 'NC_login@ajt2.local' -w 'Nextcloud001!!' -b  'dc=ajt2,dc=local' 




ldapsearch  -v -Hldap://72.69.27.130:393  -D 'mailroute@auscoinc.com' -w 'GmZ9kwV}UKHBbZ\vx@\y{G%]qy11' -b  'DC=auscoinc,DC=com' '(& (mailnickname=*)(!(mailnickname=discoverysearchmailbox*))(!(mailnickname=federatedemail*))(!(mailnickname=systemmailbox*))(!(mail=*.local)) (| (objectClass=publicFolder)(&(objectCategory=person)(objectClass=user)(!(homeMDB=*))(!(msExchHomeServerName=*)))(&(objectCategory=person)(objectClass=user)(|(homeMDB=*)(msExchHomeServerName=*)))(objectCategory=person)(objectCategory=group)(objectClass=msExchDynamicDistributionList) ) (!(objectClass=contact)) )' mail proxyAddresses




ldapsearch  -v -Hldap://mail.lcn.kmmfg.com:389  -D 'cn=kawabind,OU=LCN,O=KMMFG' -w 'NwtwC0!4!' -b  '' '(|(&(objectClass=dominoPerson)(maildomain=kmmfg)(mail=*lcn.kmmfg.com)(!(mail=*kmmfg.lcn.kmmfg.com)))(&(objectClass=dominoGroup)(maildomain=kmmfg)(mail=*lcn.kmmfg.com)(!(mail=*kmmfg.lcn.kmmfg.com))))' mail proxyAddresses 




	ldapsearch  -v -Hldap://108.39.229.102:389  -D 'mailroutesync@crl.local' -w 'KJLjkh(*&^98ugih3r098jgpoifh' -b  'DC=crl,DC=local' '(& (mailnickname=*)(!(mailnickname=discoverysearchmailbox*))(!(mailnickname=federatedemail*))(!(mailnickname=systemmailbox*))(!(mail=*.local)) (| (objectClass=publicFolder)(&(objectCategory=person)(objectClass=user)(!(homeMDB=*))(!(msExchHomeServerName=*)))(&(objectCategory=person)(objectClass=user)(|(homeMDB=*)(msExchHomeServerName=*)))(objectCategory=person)(objectCategory=group)(objectClass=msExchDynamicDistributionList) ) (!(objectClass=contact)) )' mail proxyAddresses



ldapsearch  -v -Hldaps://auth.xobeemail.net:636  -D 'CN=Mail Route,OU=Service Users,DC=bctexchange,DC=com' -w 'DADRew&f^Soqp*626VNN&Ok3l5m1@hW0' -b  'OU=BeeSweet,OU=Hosting,DC=bctexchange,DC=com' '(& (mailnickname=*)(!(mailnickname=discoverysearchmailbox*))(!(mailnickname=federatedemail*))(!(mailnickname=systemmailbox*))(!(mail=*.local)) (| (objectClass=publicFolder)(&(objectCategory=person)(objectClass=user)(!(homeMDB=*))(!(msExchHomeServerName=*)))(&(objectCategory=person)(objectClass=user)(|(homeMDB=*)(msExchHomeServerName=*)))(objectCategory=person)(objectCategory=group)(objectClass=msExchDynamicDistributionList) ) (!(objectClass=contact)) )' mail proxyAddresses


mysql> select * from amavisd_ldapsync where domain_id=16085\G
*************************** 1. row ***************************
                              id: 15157
                         enabled: 1
                         dc_list: auth.xobeemail.net
                            port: 636
                            user: CN=Mail Route,OU=Service Users,DC=bctexchange,DC=com
                        password: DADRew&f^Soqp*626VNN&Ok3l5m1@hW0
                         base_dn: OU=BeeSweet,OU=Hosting,DC=bctexchange,DC=com
                   search_string:
                      sync_count: 4
                       domain_id: 16085
                         task_id: 1838930
                          parser:
                recipient_emails: NULL
               notify_recipients: 0
                         use_ssl: 1
                       mail_attr:
                   aliases_attrs:
                exclude_patterns: NULL
     distribution_list_attribute: NULL
         distribution_list_value: NULL
                operations_limit: {"added": {}, "deleted": {}}
distribution_list_mail_attribute: NULL