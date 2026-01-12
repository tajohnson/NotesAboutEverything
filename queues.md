# cat /tmp/queues 
#!/usr/bin/perl

my ($rec, $rsn);

use HTTP::Date;

my %q = ( ' ' => 'deferred',
          '!' => 'hold  ',
          '*' => 'active' );


my $search = $ARGV[0];

print "Filtering for $search\n" if $search;

printf("%-12s%-20s%-30s%-30s\n", "Queue", "Queue-ID", "To", "From");

my $result = `dsh -x -f -t 1 -N ALL mailq 2>&1`;

my ($qid, $qfl, $siz, $dat, $from, $to);

for (split /^/, $result) {

        if (/^Mail queue is empty/) { next; }
        if (/^--/) { next; }     # print trailer
        if (/^-/) { next; }             # skip header
        # empty line
        if (/^$/) {
		if ($rsn) { $rec .= " reason=$rsn"; }
		if ( ($search && $rec !~ /$search/i) || ($rec =~ /queue=active/) ) {
               		$rec = $rsn = '';
			$qid = $qfl = $siz = $dat = $from = $to = '';
			next;
		} 
               	printf("%-12s%-20s%-30s%-30s\n%-12s%s\n", $q{$qfl}, $qid, $to, $from, "", $rsn);
                $rec = $rsn = '';
		$qid = $qfl = $siz = $dat = $from = $to = '';
                next;
        }
        # line with queue id
        #if (/^(\S+)\s*([ !*])\s+(\d+)\s+(\S+\s+\S+\s+\d+\s+\d+:\d+:\d+)\s+(.*)$/)
        if (/^([a-zA-Z0-9]+)([ !*])\s+(\d+)\s+(\S+\s+\S+\s+\d+\s+\d+:\d+:\d+)\s+(.*)$/)
        {
                ($qid, $qfl, $siz, $dat, $from) = ($1, $2, $3, $4, $5);
		
                $dat = HTTP::Date::time2isoz(str2time($dat));
                $dat =~ s/ /T/g;
                $siz = sprintf "%08d", $siz;
                $rec="$qid queue=$q{$qfl} size=$siz date=$dat from=$from";
                next;
        }
        #if (/^\s*\((.+)\)$/) { $rsn = $1; $rsn =~ tr/ /_/; next; }
        if (/^\s*\((.+)\)$/) { $rsn = $1; next; }
        if (/^\s+(.+)$/) { $to = " $1"; $rec .= " to=$to"; next; }  # to:
}

