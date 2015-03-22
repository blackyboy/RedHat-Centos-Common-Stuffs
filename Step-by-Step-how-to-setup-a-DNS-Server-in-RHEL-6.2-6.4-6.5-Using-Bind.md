Step by Step how to setup a DNS Server in RHEL 6.2/6.4/6.5 Using Bind

What is DNS Server ?

DNS = Domain Naming Service (or) Domain Name System
DNS will resolve the host name for the particular IP address.


Here Im Using RHEL Server to Setup the DNS Server using BIND

```
[root@masterdns ~]# lsb_release -a
LSB Version:	:core-4.0-amd64:core-4.0-noarch:graphics-4.0-amd64:graphics-4.0-noarch:printing-4.0-amd64:printing-4.0-noarch
Distributor ID:	RedHatEnterpriseServer
Description:	Red Hat Enterprise Linux Server release 6.2 (Santiago)
Release:	6.2
Codename:	Santiago
```

Primary DNS Server (or) Master DNS Server:

```
IP Address :	192.168.0.200
Hostname   :	masterdns.linuxzadmin.local
```

Secondary DNS Server (or) Slave DNS Server:

```
IP Address :	192.168.0.201
Hostname   :	slavedns.linuxzadmin.local
```

Nodes Machines :

```
IP Address : 	192.168.0.205  ## Hostname : node1.linuxzadmin.local
IP Address : 	192.168.0.206  ## Hostname : node2.linuxzadmin.local
IP Address : 	192.168.0.207  ## Hostname : node3.linuxzadmin.local
IP Address : 	192.168.0.208  ## Hostname : node4.linuxzadmin.local
```

1. Primary DNS Server (or) Master DNS Server :

```
[root@masterdns ~]# yum install bind* -y
```

2. Then Edit the Configuration of name server

```
[root@masterdns ~]# vim /etc/named.conf 

//
// named.conf
//
// Provided by Red Hat bind package to configure the ISC BIND named(8) DNS
// server as a caching only nameserver (as a localhost DNS resolver only).
//
// See /usr/share/doc/bind*/sample/ for example named configuration files.
//
options {
	listen-on port 53 { 127.0.0.1; 192.168.0.200; }; # Master DNS Servers IP
	listen-on-v6 port 53 { ::1; };
	directory 	"/var/named";
	dump-file 	"/var/named/data/cache_dump.db";
        statistics-file "/var/named/data/named_stats.txt";
        memstatistics-file "/var/named/data/named_mem_stats.txt";
	allow-query     { localhost; 192.168.0.0/24; }; # IP Range of Hosts
	allow-transfer  { localhost; 192.168.0.201; }; # Slave DNS Servers IP
	recursion yes;

	dnssec-enable yes;
	dnssec-validation yes;
	dnssec-lookaside auto;

	/* Path to ISC DLV key */
	bindkeys-file "/etc/named.iscdlv.key";
	managed-keys-directory "/var/named/dynamic";
};

logging {
        channel default_debug {
                file "data/named.run";
                severity dynamic;
        };
};

zone "." IN {
	type hint;
	file "named.ca";
};
zone"linuxzadmin.local" IN {
type master;
file "forward.linuxzadmin";
allow-update { none; };
};
zone"0.168.192.in-addr.arpa" IN {
type master;
file "reverse.linuxzadmin";
allow-update { none; };
};
include "/etc/named.rfc1912.zones";
include "/etc/named.root.key";
```

Save and Exit the named.conf using wq!


3. Creat the Forward and Reserve Zone files as mentioned in named.conf

### FORWARD ZONE :
------------

a.) Create a Forward Zone file under /var/named in the name of forward.linuxzadmin

There are Sample files under the /var/named/ Directory, Just make a Copy of that file and modify it as our need


b.) Make a Copy of sample file as below

```
[root@masterdns ~]# cp /var/named/named.localhost /var/named/forward.linuxzadmin
```

c.) Edit the file forward.linuxzadmin

```
[root@masterdns ~]# vim /var/named/forward.linuxzadmin


$TTL 86400
@       IN SOA  masterdns.linuxzadmin.local. root.linuxzadmin.local. (
                                2014051001      ; serial
                                        3600    ; refresh
                                        1800    ; retry
                                        604800  ; expire
                                        86400   ; minimum
)
@               IN      NS      masterdns.linuxzadmin.local.
@               IN      NS      slavedns.linuxzadmin.local.
@               IN      A       192.168.0.200
@               IN      A       192.168.0.201
@               IN      A       192.168.0.205
@               IN      A       192.168.0.206
@               IN      A       192.168.0.207
@               IN      A       192.168.0.208
masterdns       IN      A       192.168.0.200
slavedns        IN      A       192.168.0.201
node1           IN      A       192.168.0.205
node2           IN      A       192.168.0.206
node3           IN      A       192.168.0.207
node4           IN      A       192.168.0.208
```

### RESERVE ZONE:
------------

a.) Create a Reserver Zone file under /var/named in the name of reverse.linuxzadmin

There are Sample files under the /var/named/ Directory, Just make a Copy of that file and modify it as our need

b.) Make a Copy of sample file as below

```
[root@masterdns ~]# cp /var/named/named.loopback /var/named/reverse.linuxzadmin

````

c.) Edit the file reverse.linuxzadmin

```
[root@masterdns ~]# vim /var/named/reverse.linuxzadmin 


$TTL 86400
@       IN SOA  masterdns.linuxzadmin.local. root.linuxzadmin.local. (
                                2014051001      ; serial
                                        3600    ; refresh
                                        1800    ; retry
                                        604800  ; expire
                                        86400   ; minimum
)
@               IN      NS      masterdns.linuxzadmin.local.
@               IN      NS      slavedns.linuxzadmin.local.
@               IN      PTR     linuxzadmin.local.
masterdns       IN      A       192.168.0.200
slavedns        IN      A       192.168.0.201
node1           IN      A       192.168.0.205
node2           IN      A       192.168.0.206
node3           IN      A       192.168.0.207
node4           IN      A       192.168.0.208
200             IN      PTR     masterdns.linuxzadmin.local.
201             IN      PTR     slavedns.linuxzadmin.local.
205             IN      PTR     node1.linuxzadmin.local.
206             IN      PTR     node2.linuxzadmin.local.
207             IN      PTR     node3.linuxzadmin.local.
208             IN      PTR     node4.linuxzadmin.local.
```

5. The files we created was in root group
    We need to change those files to named group

Here we can see the files which have the root group

a.) List the files and see the permissions and group of those created zone files

```
[root@masterdns ~]# ls -l /var/named/
total 40
drwxr-x---. 6 root  named 4096 May 10 19:33 chroot
drwxrwx---. 2 named named 4096 Nov 16  2011 data
drwxrwx---. 2 named named 4096 Nov 16  2011 dynamic
-rw-r-----. 1 root  root   550 May 10 20:19 forward.linuxzadmin
-rw-r-----. 1 root  named 1892 Feb 18  2008 named.ca
-rw-r-----. 1 root  named  152 Dec 15  2009 named.empty
-rw-r-----. 1 root  named  152 Jun 21  2007 named.localhost
-rw-r-----. 1 root  named  168 Dec 15  2009 named.loopback
-rw-r-----. 1 root  root   676 May 10 20:35 reverse.linuxzadmin
drwxrwx---. 2 named named 4096 Nov 16  2011 slaves
```

b.) Change the group to named using below Command 

```
[root@masterdns ~]# chgrp named /var/named/forward.linuxzadmin 
[root@masterdns ~]# chgrp named /var/named/reverse.linuxzadmin 
```

Here we can see the Output now which changed to named group

```
[root@masterdns ~]# ls -l /var/named/
total 40
drwxr-x---. 6 root  named 4096 May 10 19:33 chroot
drwxrwx---. 2 named named 4096 Nov 16  2011 data
drwxrwx---. 2 named named 4096 Nov 16  2011 dynamic
-rw-r-----. 1 root  named  550 May 10 20:19 forward.linuxzadmin
-rw-r-----. 1 root  named 1892 Feb 18  2008 named.ca
-rw-r-----. 1 root  named  152 Dec 15  2009 named.empty
-rw-r-----. 1 root  named  152 Jun 21  2007 named.localhost
-rw-r-----. 1 root  named  168 Dec 15  2009 named.loopback
-rw-r-----. 1 root  named  676 May 10 20:35 reverse.linuxzadmin
drwxrwx---. 2 named named 4096 Nov 16  2011 slaves
```

c.) Then we need to check the Context of the files under

```
[root@masterdns ~]# ls -lZd /etc/named.conf 
-rw-r-----. root named system_u:object_r:named_conf_t:s0 /etc/named.conf

/etc/named.conf
/var/named/forward.linuxzadmin
/var/named/reverse.linuxzadmin
```

It want to be in the context of named_conf_t

If its Different than this then we need to restore the context using 

```
# restorecon /etc/named.conf 
```

6. Now we need to Check for the Error in the conf file and Zone file

```
[root@masterdns ~]# named-checkconf /etc/named.conf 

[root@masterdns ~]# named-checkzone linuxzadmin.local /var/named/forward.linuxzadmin 
zone linuxzadmin.local/IN: loaded serial 2014051001
OK

[root@masterdns ~]# named-checkzone 0.168.192.in-addr.arpa /var/named/reverse.linuxzadmin 
zone 0.168.192.in-addr.arpa/IN: loaded serial 2014051001
OK
```

7. Start the DNS Service 

```
[root@masterdns ~]# service named restart
Stopping named:                                            [  OK  ]
Starting named:                                            [  OK  ]
```

8. Make the named Service in runlevels

```
[root@masterdns ~]# chkconfig named on

[root@masterdns ~]# chkconfig --list named
named          	0:off	1:off	2:on	3:on	4:on	5:on	6:off
```

9. Deploy iptables Rules to allow DNS service 

Add the iptables rules 


```
iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -A INPUT -p tcp -m state --state NEW -m tcp --dport 22 -j ACCEPT
iptables -A INPUT -p tcp -m state --state NEW -m tcp --dport 53 -j ACCEPT
iptables -A INPUT -p udp -m state --state NEW -m udp --dport 53 -j ACCEPT
iptables -A INPUT -j DROP
```

Save the iptables Using 

```
[root@masterdns ~]# service iptables save
iptables: Saving firewall rules to /etc/sysconfig/iptables:[  OK  ]
```

Restart the iptables Service Using 


```
[root@masterdns ~]# service iptables restart
iptables: Flushing firewall rules:                         [  OK  ]
iptables: Setting chains to policy ACCEPT: filter          [  OK  ]
iptables: Unloading modules:                               [  OK  ]
iptables: Applying firewall rules:                         [  OK  ]
```

Make it to run in multi run levels

```
[root@masterdns ~]# chkconfig iptables on

[root@masterdns ~]# chkconfig --list iptables 
iptables       	0:off	1:off	2:on	3:on	4:on	5:on	6:off
```

10. Check the DNS server using Dig Command


```
[root@masterdns ~]# dig masterdns.linuxzadmin.local

; <<>> DiG 9.7.3-P3-RedHat-9.7.3-8.P3.el6 <<>> masterdns.linuxzadmin.local
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 41316
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 2, ADDITIONAL: 1

;; QUESTION SECTION:
;masterdns.linuxzadmin.local.	IN	A

;; ANSWER SECTION:
masterdns.linuxzadmin.local. 86400 IN	A	192.168.0.200

;; AUTHORITY SECTION:
linuxzadmin.local.	86400	IN	NS	masterdns.linuxzadmin.local.
linuxzadmin.local.	86400	IN	NS	slavedns.linuxzadmin.local.

;; ADDITIONAL SECTION:
slavedns.linuxzadmin.local. 86400 IN	A	192.168.0.201

;; Query time: 0 msec
;; SERVER: 192.168.0.200#53(192.168.0.200)
;; WHEN: Sat May 10 23:07:10 2014
;; MSG SIZE  rcvd: 114
```

11. Check for the Available Hosts in DNS


```
[root@masterdns ~]# nslookup linuxzadmin.local
Server:		192.168.0.200
Address:	192.168.0.200#53

Name:	linuxzadmin.local
Address: 192.168.0.207
Name:	linuxzadmin.local
Address: 192.168.0.208
Name:	linuxzadmin.local
Address: 192.168.0.200
Name:	linuxzadmin.local
Address: 192.168.0.201
Name:	linuxzadmin.local
Address: 192.168.0.205
Name:	linuxzadmin.local
Address: 192.168.0.206
```

Now we Need to Setup the Slave DNS server 


### Secondary DNS server (or) Slave DNS Server

1. Host Deployed with RHEL Server

```
[root@slavedns ~]# lsb_release -a
LSB Version:	:core-4.0-amd64:core-4.0-noarch:graphics-4.0-amd64:graphics-4.0-noarch:printing-4.0-amd64:printing-4.0-noarch
Distributor ID:	RedHatEnterpriseServer
Description:	Red Hat Enterprise Linux Server release 6.2 (Santiago)
Release:	6.2
Codename:	Santiago
```

2. Insatall the BIND package in Server

```
[root@slavedns ~]# yum install bind* -y
```

3. Edit the named.conf to add the configuration

```
[root@slavedns ~]# vim /etc/named.conf 


//
// named.conf
//
// Provided by Red Hat bind package to configure the ISC BIND named(8) DNS
// server as a caching only nameserver (as a localhost DNS resolver only).
//
// See /usr/share/doc/bind*/sample/ for example named configuration files.
//

options {
	listen-on port 53 { 127.0.0.1; 192.168.0.201; }; # Slave DNS server's IP
	listen-on-v6 port 53 { ::1; };
	directory 	"/var/named";
	dump-file 	"/var/named/data/cache_dump.db";
        statistics-file "/var/named/data/named_stats.txt";
        memstatistics-file "/var/named/data/named_mem_stats.txt";
	allow-query     { localhost; 192.168.0.0/24; };
	recursion yes;

	dnssec-enable yes;
	dnssec-validation yes;
	dnssec-lookaside auto;

	/* Path to ISC DLV key */
	bindkeys-file "/etc/named.iscdlv.key";
	managed-keys-directory "/var/named/dynamic";
};

logging {
        channel default_debug {
                file "data/named.run";
                severity dynamic;
        };
};

zone "." IN {
	type hint;
	file "named.ca";
};
zone"linuxzadmin.local" IN {
type slave;
file "slaves/linuxzadmin.fwd";
masters { 192.168.0.200; };
};
zone"0.168.192.in-addr.arpa" IN {
type slave;
file "slaves/linuxzadmin.rev";
masters { 192.168.0.200; };
};
include "/etc/named.rfc1912.zones";
include "/etc/named.root.key";
```

4. Start the named Service and make it to Run in Multi Runlevels

```
[root@slavedns ~]# service named start 
Starting named:                                            [  OK  ]

[root@slavedns ~]# chkconfig named on

[root@slavedns ~]# chkconfig --list named
named          	0:off	1:off	2:on	3:on	4:on	5:on	6:off
```

5. We Don't need to Create the Zone file here, If will be resolved from Master Server While we Start the Named Service 

```
[root@slavedns ~]# ls -l /var/named/slaves/
total 8
-rw-r--r--. 1 named named 634 May 10 23:35 linuxzadmin.fwd
-rw-r--r--. 1 named named 773 May 10 23:35 linuxzadmin.rev
```

6. Here we can Check the Both File's 

```
[root@slavedns ~]# cat /var/named/slaves/linuxzadmin.fwd 
$ORIGIN .
$TTL 86400	; 1 day
linuxzadmin.local	IN SOA	masterdns.linuxzadmin.local. root.linuxzadmin.local. (
				2014051001 ; serial
				3600       ; refresh (1 hour)
				1800       ; retry (30 minutes)
				604800     ; expire (1 week)
				86400      ; minimum (1 day)
				)
			NS	slavedns.linuxzadmin.local.
			NS	masterdns.linuxzadmin.local.
			A	192.168.0.200
			A	192.168.0.201
			A	192.168.0.205
			A	192.168.0.206
			A	192.168.0.207
			A	192.168.0.208
$ORIGIN linuxzadmin.local.
masterdns		A	192.168.0.200
node1			A	192.168.0.205
node2			A	192.168.0.206
node3			A	192.168.0.207
node4			A	192.168.0.208
slavedns		A	192.168.0.201
```

This is the Out put of linuxzadmin.rev 

```
[root@slavedns ~]# cat /var/named/slaves/linuxzadmin.rev 
$ORIGIN .
$TTL 86400	; 1 day
0.168.192.in-addr.arpa	IN SOA	masterdns.linuxzadmin.local. root.linuxzadmin.local. (
				2014051001 ; serial
				3600       ; refresh (1 hour)
				1800       ; retry (30 minutes)
				604800     ; expire (1 week)
				86400      ; minimum (1 day)
				)
			NS	slavedns.linuxzadmin.local.
			NS	masterdns.linuxzadmin.local.
			PTR	linuxzadmin.local.
$ORIGIN 0.168.192.in-addr.arpa.
200			PTR	masterdns.linuxzadmin.local.
201			PTR	slavedns.linuxzadmin.local.
205			PTR	node1.linuxzadmin.local.
206			PTR	node2.linuxzadmin.local.
207			PTR	node3.linuxzadmin.local.
208			PTR	node4.linuxzadmin.local.
masterdns		A	192.168.0.200
node1			A	192.168.0.205
node2			A	192.168.0.206
node3			A	192.168.0.207
node4			A	192.168.0.208
slavedns		A	192.168.0.201
```

7. Check the DNS Server using dig from Slave Server 

```
[root@slavedns ~]# dig masterdns.linuxzadmin.local

; <<>> DiG 9.7.3-P3-RedHat-9.7.3-8.P3.el6 <<>> masterdns.linuxzadmin.local
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 11178
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 2, ADDITIONAL: 1

;; QUESTION SECTION:
;masterdns.linuxzadmin.local.	IN	A

;; ANSWER SECTION:
masterdns.linuxzadmin.local. 86400 IN	A	192.168.0.200

;; AUTHORITY SECTION:
linuxzadmin.local.	86400	IN	NS	masterdns.linuxzadmin.local.
linuxzadmin.local.	86400	IN	NS	slavedns.linuxzadmin.local.

;; ADDITIONAL SECTION:
slavedns.linuxzadmin.local. 86400 IN	A	192.168.0.201

;; Query time: 2 msec
;; SERVER: 192.168.0.200#53(192.168.0.200)
;; WHEN: Sat May 10 23:42:03 2014
;; MSG SIZE  rcvd: 114
```

### Client Side :


8. Now we Need to Assign the Name Server for the Node's in our network to get assigned a host name from DNS server.


Use the Setup Command and assign the Primary and Secondary DNS server's
We Don't need to Assing the hostname

a.) Just Assign the IP, Subnet, Gateway, PDNS, SDNS

b.) Restart the Network and Check the hostname


c.) Here i have not changed the hostname


```
[root@node1 ~]# cat /etc/sysconfig/network
NETWORKING=yes
HOSTNAME=localhost.localdomain
```

d.) Here we can see the hostname Assigned from the DNS server

```
[root@node1 ~]# hostname
node1.linuxzadmin.local
```

e.) If we need to check the DNS just do a Dig 


```
[root@node1 ~]# dig masterdns.linuxzadmin.local

; <<>> DiG 9.7.3-P3-RedHat-9.7.3-8.P3.el6 <<>> masterdns.linuxzadmin.local
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 51788
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 2, ADDITIONAL: 1

;; QUESTION SECTION:
;masterdns.linuxzadmin.local.	IN	A

;; ANSWER SECTION:
masterdns.linuxzadmin.local. 86400 IN	A	192.168.0.200

;; AUTHORITY SECTION:
linuxzadmin.local.	86400	IN	NS	slavedns.linuxzadmin.local.
linuxzadmin.local.	86400	IN	NS	masterdns.linuxzadmin.local.

;; ADDITIONAL SECTION:
slavedns.linuxzadmin.local. 86400 IN	A	192.168.0.201

;; Query time: 1 msec
;; SERVER: 192.168.0.200#53(192.168.0.200)
;; WHEN: Sat May 10 23:58:32 2014
;; MSG SIZE  rcvd: 114
```

9. If we need to flush the DNS Server Caches Use Below Command

```
# yum install nscd
# nscd -i hosts
```

That's it we have a DNS server now in RHEL Server

