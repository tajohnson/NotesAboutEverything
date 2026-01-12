
#!/usr/bin/perl

use strict;
use warnings;

my $email;
while (<>) {
 $email .= $_;
}

my ($boundary) = $email =~ /boundary="(.*)"/;
my ($orig_content) = $email =~ /^--$boundary.*^--$boundary(.*)$boundary--/ms;

print $orig_content;

You would use it like this:

./spam_extractor.pl < email_file_with_encapsualted_spam