Do I need to provide network cables?

Get enough power cables!

Check to be sure each server has all the rack hardware, including screws.




Shipping to Miami:
	4 servers
	1 juniper EX3300 switch
	2 HP ProCurve 2848 switches
	
Juniper Ex3300-48T is for public network.
One HP ProCurve 2848 is for private network.  
The other 2848 is for IPMI.  


Rack and Cabling instructions.

 Between the boxes the servers and switches come in, and the extra box of cables, I think you'll have all the cabling you need.

 1. The Juniper EX3300-48T is coming to you directly from the supplier, and needs an IP address assigned.  Can you please do a basic configuration so I can access it and configure it remotely?
 
    a. Can you put a label on it, front and back, "sw01.mia.mailroute.net"?
 
    b. I'm 99.999% certain you don't need this, but here's some info on the initial setup: https://www.juniper.net/documentation/en_US/release-independent/junos/topics/task/configuration/ex-series-initial-configuration-setting-up-cli.html
 
    c. Please set to root password to "mailRoute1298"
 
    d. Enable ssh
 
    e. Set the management IP address to 199.89.3.245, gateway address to 199.89.3.1
 
 2. Please rack the switches and servers. Power, of course :)  
 
 3. Please connect the public drop to port 0 on the Juniper Ex3300T. 
 
 4. Please connect the private drop to port 0 on the HP switch labeled pr01.  
 
 5. Please connect port 0 on the HP switch labeled IPMI01 to port 47 of the Juniper switch.  Please use a white cable, if you have it.

 
 6. Network connections - just start with port1 on each switch, if you would.
    a. IPMI ports on each server to the IPMI switch with red cable
    b. eth0 to the public switch with green cable
    c. eth1 to the private switch with yellow cable
     
 
 Note: 
    Besides the IPMI port, there are 4 ethernet ports on each server.  
    The public eth0 port is the one on the bottom left.  
    The private eth1 port is on the bottom right.
 
 
     Here are the locations of the ports on the servers:

             IPMI
             USB1  USB3  eth2 eth3
      COM1   USB0  USB2  eth0 eth1   VGA
      

And then please provide pictures of the rack, clearly showing the fronts and backs of the servers.  Just email them to "tj@mailroute.net".

Thank you-

Tom
    



