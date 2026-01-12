# imapsync

/usr/bin/imapsync --host1 imap.islandemail.com --user1 tj@terramar.net --password1 MASKED --host2 localhost --user2 user01@mailroute.email --password2 password --maxsize  5242880000



https://github.com/imapsync/imapsync


cpanm install App::cpanminus Authen::NTLM CGI Data::Uniqid Dist::CheckConflicts Encode::IMAPUTF7 File::Copy::Recursive File::Tail IO::Socket::INET6 IO::Tee JSON::WebToken JSON::WebToken::Crypt::RSA LWP::UserAgent Mail::IMAPClient Module::ScanDeps Package::Stash Package::Stash::XS PAR::Packer Parse::RecDescent Proc::ProcessTable Readonly Regexp::Common Sys::MemInfo Test::Mock::Guard Test::MockObject Test::Pod Test::Deep Unicode::String



What about imapsync's --maxage flag?
If you're going to have repeated runs on imapsync it seems that you can shrink the time for each sync by setting --maxage to the number of days between the start and end of the previous imapsync.
I did an early test. It took me 50 minutes to imapsync 450M of mail even if run repeatedly every hour. However if I run a second time with "--maxage 1", i.e. sync only messages from the last day, then it took only 3 minutes. This is only for a single user but I expect this trick to ease my migration for thousands of users.
I expect the first sync to take a long time: let's assume n days. After that I would sync again but with maxage at n and then I expect n to decrease for each call.
