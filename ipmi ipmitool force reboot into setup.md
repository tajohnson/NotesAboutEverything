ipmitool -H 199.89.1.141 -U tjohnson -P windowfollowmonster  raw 0x00 0x08 0x05 0x80 0x18 0x00 0x00 0x00



user: monitor
password: farthermovingeaster


ipmitool -H 199.89.1.132 -U tjohnson -P windowfollowmonster  user set name 4 monitor
ipmitool -H 199.89.1.132 -U tjohnson -P windowfollowmonster  user set password 4

ipmitool -H 199.89.1.132 -U tjohnson -P windowfollowmonster  channel setaccess 1 4 link=on ipmi=on callin=on privilege=2
ipmitool -H 199.89.1.132 -U tjohnson -P windowfollowmonster  user enable 4
ipmitool -H 199.89.1.132 -U tjohnson -P windowfollowmonster  channel getaccess 1 4
