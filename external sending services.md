
CREATE TABLE external_sending_services (
  id int(11) NOT NULL AUTO_INCREMENT,
  name varchar(255) DEFAULT NULL,
  url varchar(255) DEFAULT NULL,
  category varchar(20) DEFAULT NULL,
  spf_include varchar(255) DEFAULT NULL,
  cidr_blocks blob,
  created_at timestamp NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at timestamp NULL DEFAULT '0000-00-00 00:00:00' ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (id),
  KEY(category),
  KEY(url),
  UNIQUE KEY(spf_include),
  UNIQUE KEY(url)
) ENGINE=InnoDB  DEFAULT CHARSET=utf8;

insert into external_sending_services values (NULL, 'Postmark', 'postmamarkapp.com', 'Transactional', 'pf.mtasv.net', NULL, NOW(), NOW());
insert into external_sending_services values (NULL, 'MailGun', 'mailgun.com', 'Transactional', 'mailgun.org', NULL, NOW(), NOW());
insert into external_sending_services values (NULL, 'MailJet', 'mailjet.com', 'Transactional', 'spf.mailjet.com', NULL, NOW(), NOW());
insert into external_sending_services values (NULL, 'Amazon SES', 'amazonses.com', 'Transactional', 'amazonses.com', NULL, NOW(), NOW());
insert into external_sending_services values (NULL, 'ElasitcEMail', 'elasticemail.com', 'Transactional', '_spf.elasticemail.com', NULL, NOW(), NOW());
insert into external_sending_services values (NULL, 'PepiPost', 'pepipost.com', 'Transactional', 'pepipost.net', NULL, NOW(), NOW());

insert into external_sending_services values (NULL, "MailChimp","mailchimp.com", "Bulk", "servers.mcsv.net", NULL, NOW(), NOW());
insert into external_sending_services values (NULL, "ActiveCampaign", "activecampaign.com", "Bulk", "emsd1.com", NULL, NOW(), NOW());
insert into external_sending_services values (NULL, "MailerLite", "mailerlite.com", "Bulk", "_spf.mlsend.com", NULL, NOW(), NOW());
insert into external_sending_services values (NULL, "GetResponse", "getresponse.com", "Bulk", "_spf.getresponse.com", NULL, NOW(), NOW());
insert into external_sending_services values (NULL, "iContact", "icontact.com", "Bulk", "icpbounce.com", NULL, NOW(), NOW());
insert into external_sending_services values (NULL, "OntraMail", "ontramail.com", "Bulk", "_spf-ontramail.ontramail.com", NULL, NOW(), NOW());
insert into external_sending_services values (NULL, "Campaign Monitor", "compaignmonitor", "Bulk", "_spf.createsend.com", NULL, NOW(), NOW());
insert into external_sending_services values (NULL, "Constant Contact", "constantcontact.com", "Bulk", "spf.constantcontact.com", NULL, NOW(), NOW());

insert into external_sending_services values (NULL, "Sendinblue", "sendinblue.com", "Bulk",NULL, '77.32.128.0/18
77.32.192.0/19
185.41.28.0/22
185.107.232.0/22
94.143.16.0/21
185.24.144.0/22
153.92.224.0/19
213.32.128.0/18
212.146.192.0/18
172.246.0.0/16', NOW(), NOW());


insert into external_sending_services values (NULL, "Sendinblue", "sendinblue.com", "Bulk", NULL, '77.32.128.0/18
77.32.192.0/19
185.41.28.0/22
185.107.232.0/22
94.143.16.0/21
185.24.144.0/22
153.92.224.0/19
213.32.128.0/18
212.146.192.0/18
172.246.0.0/16', NOW(), NOW());

insert into external_sending_services values (NULL, "SendGrid", "sendgrid.com", "Bulk", NULL, '149.72.100.0/23
149.72.102.0/23
149.72.1.0/24	
149.72.104.0/23
149.72.112.0/20
149.72.12.0/22	
149.72.128.0/19
149.72.160.0/19
149.72.19.0/24	
149.72.192.0/19
149.72.20.0/22	
149.72.2.0/23	
149.72.224.0/19
149.72.24.0/21	
149.72.32.0/19	
149.72.4.0/22	
149.72.64.0/19
149.72.8.0/22
149.72.96.0/23
149.72.98.0/23
159.183.128.0/18
159.183.192.0/19
167.89.0.0/18
167.89.112.0/24
167.89.113.0/24
167.89.114.0/23
167.89.116.0/24
167.89.117.0/24
167.89.118.0/24
167.89.119.0/24
167.89.120.0/23
167.89.122.0/23
167.89.124.0/23
167.89.126.0/23
167.89.64.0/19
167.89.96.0/20
168.245.0.0/18
168.245.64.0/20
168.245.80.0/22
168.245.84.0/22
168.245.88.0/22
168.245.92.0/22
168.245.96.0/19
192.254.112.0/20
198.21.0.0/21
198.37.144.0/20
208.117.48.0/21
208.117.56.0/21
223.165.113.0/24
223.165.115.0/24
223.165.118.0/24
223.165.119.0/24
223.165.120.0/24
223.165.121.0/24
50.31.32.0/20
50.31.48.0/20', NOW(), NOW());



# - adding social media sites too
insert into external_sending_services values (NULL, 'Classmates', 'classmates.com', 'Social', 'pf.mtasv.net', NULL, NOW(), NOW());
insert into external_sending_services values (NULL, 'FaceBook', 'facebook.com', 'Social', 'pf.mtasv.net', NULL, NOW(), NOW());
insert into external_sending_services values (NULL, 'FetLife', 'fetlife.com', 'Social', 'pf.mtasv.net', NULL, NOW(), NOW());
insert into external_sending_services values (NULL, 'Flickr', 'flickr.com', 'Social', 'pf.mtasv.net', NULL, NOW(), NOW());
insert into external_sending_services values (NULL, 'Instagram', 'instagram.com', 'Social', 'pf.mtasv.net', NULL, NOW(), NOW());
insert into external_sending_services values (NULL, 'LinkedIn', 'linkedin.com', 'Social', 'pf.mtasv.net', NULL, NOW(), NOW());
insert into external_sending_services values (NULL, 'MySpace', 'myspace.com', 'Social', 'pf.mtasv.net', NULL, NOW(), NOW());
insert into external_sending_services values (NULL, 'NextDoor', 'nextdoor.com', 'Social', 'pf.mtasv.net', NULL, NOW(), NOW());
insert into external_sending_services values (NULL, 'Parler', 'parler.com', 'Social', 'pf.mtasv.net', NULL, NOW(), NOW());
insert into external_sending_services values (NULL, 'Pinterest', 'pinterest.com', 'Social', 'pf.mtasv.net', NULL, NOW(), NOW());
insert into external_sending_services values (NULL, 'Reddit', 'parler.com', 'Social', 'pf.mtasv.net', NULL, NOW(), NOW());
insert into external_sending_services values (NULL, 'Parler', 'parler.com', 'Social', 'pf.mtasv.net', NULL, NOW(), NOW());


pinterest.com
reddit.com
snapchat.com
tiktok.com
truthsocial.com
twitter.com




# cat mr_services_generate_ip.pl 
#!/usr/bin/perl

use DBI;
use Data::Dumper;

$sql_select  = "SELECT name,category,spf_include,cidr_blocks from external_sending_services";


$SQL{'dbtype'} = "mysql";   # Database type (MySQL,DBM,PostgreSQL,...)
$SQL{'user'}   = "postfix_admin";     # Username
$SQL{'pass'}   = "jumpstructurechildrengolden";     # Password
$SQL{'dbase'}  = "mroute_admin";    # Database
$SQL{'host'} = "db-postfix-ro.mailroute.net",
$dbh = DBI->connect("DBI:".$SQL{'dbtype'}.":".$SQL{'dbase'} . ":" . $SQL{'host'},$SQL{'user'},$SQL{'pass'});

if (!$dbh) {
        exit;
}

sub process_spf {
  my $spf = shift;
  my $lookup  = `dig +short txt $spf`;
  my @a = split(/\n/, $lookup);
  foreach my $i (@a) {
    $i =~ tr/"//d;
    next unless $i =~ /v=spf/;
    my @b = split (/ /, $i);
    foreach my $j (@b) {
      my ($type, $val) = split(/:/, $j);
      if ($type =~ /^include/) {
        process_spf($val);
      } elsif ($type =~ /^ip4/) {
        print $val . "\n";
      } elsif ($type =~ /^ip6/) {
      }
    }
  }
}


# print the header

print <<EOF;
# 5m localhost. hostmaster.localhost. 1402132225 1h 10m 5d 30s
# 300s
EOF



# loop through all the providers

my $sth1 = $dbh->prepare($sql_select);
$sth1->execute();

my @r;
while (@r = $sth1->fetchrow_array) {
    my ($name,$category,$spf_include,$cidr_blocks) = @r;

    my $return_code = ($category =~ /bulk/i ) ? "127.0.0.4" : (($category =~ /transactional/i) ? "127.0.0.2" : "127.0.0.6");

    printf(":%s:%s Email Provider - %s\n", $return_code, $category, $name);
    foreach my $a (split(/ /, $spf_include)) {
        process_spf($a);
    }
    print $cidr_blocks . "\n" if $cidr_blocks;
    
}
