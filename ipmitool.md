https://www.thomas-krenn.com/en/wiki/Configuring_IPMI_under_Linux_using_ipmitool



modprobe ipmi_devintf
modprobe ipmi_si


ipmitool lan set 1 ipaddr 199.89.1.144
ipmitool lan set 1 netmask 255.255.255.0
ipmitool lan set 1 defgw ipaddr 199.89.1.1


ipmitool user set name 2 admin
ipmitool user set password 2
ipmitool channel setaccess 1 2 link=on ipmi=on callin=on privilege=4
ipmitool user enable 2

ipmitool user set name 3 tjohnson
ipmitool user set password 3
ipmitool channel setaccess 1 3 link=on ipmi=on callin=on privilege=4
ipmitool user enable 3
