You could use something like the following in amavisd.conf,
assuming you would add a field 'retention' into a policy SQL table,
where the field would be a numeric retention time interval in seconds,
or a NULL for a built-in default.


BEGIN { import Amavis::Util qw(min max) }

$partition_tag = sub {
 my($msginfo) = @_;
 my $ret_dflt = 2 * 7*24*3600;  # two weeks by default
 # retention time interval in seconds, max across all recepients of a message
 my $retention =
   max( map { my($ret,$mk) = Amavis::Lookup::lookup2(0, $_->recip_addr,
                               [ q_sql_n('retention'), $ret_dflt ],
                               Label => 'retention');
              $ret;
            } @{$msginfo->per_recip_data});
 $retention = min(90*24*3600, max(24*3600, $retention)); # clip to 1..90 days
 sprintf("%02d", iso8601_week($msginfo->rx_time + $retention));
};

