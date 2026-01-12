start smbd on 199.89.0.115


share host: 199.89.0.115
path to image: \install\supermicrobios.iso
user: shareuser
password: kate8kate




 1182  wget http://freshrpms.net/docs/bios-flash/fdboot.img.bz2
 1186  bunzip2 /tmp/fdboot.img.bz2 
 1189  cp /tmp/fdboot.img /tmp/t
 1190  mksirofs -r -b fdboot.img -c boot.catalog -o tj.iso /tmp/t
 1191  mksisofs -r -b fdboot.img -c boot.catalog -o tj.iso /tmp/t
 1192  mkisofs -r -b fdboot.img -c boot.catalog -o tj.iso /tmp/t

