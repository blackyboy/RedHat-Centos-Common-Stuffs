There are 2 Ethernet card's

1. eth0 with 192.168.1.0/24 connected to ISP router @ 192.168.1.1, Assigned With IP 192.168.1.15

2. eth1 with network 198.168.0.0/24 DHCP server for local network here, or With Static IP's, 192.168.0.15

3. iptable with the basic rules including the squid3 redirect rules to redirects.

4. Both Ethernet card's need to Assign With Static IP Address in differnet network.

Create the list of Blocked files under 

```
# vim /etc/squid/blockedsites.squid
```

Enter the needed sites need to get block

```
.facebook.com
.skypeassets.com
.maalaimalar.com
.ui.skype.com
.twitter.com
.ndtv.com
```

Create the list of Blocked Keywords

```
# vim /etc/squid/blockkeywords.squid
```
porn

Create the list of Blocked IP's

```
# vim /etc/squid/blockip.squid
```

192.168.0.212

Create the list of Allowed IP's

```
# vim /etc/squid/allowip.squid
```

192.168.0.10

Edit the Configuration file and Enter the Below Rule's

```
# vim /etc/squid/squid.conf
```

Change to the Respective Needed Rules 

```
#########################################################################
##### Restricting Web Access By Time #####
acl home_network src 192.168.0.0/24
acl business_hours time M T W H F A 08:45-22:00
acl RestrictedHost src 192.168.0.221
# Restricting Web Access By Time
http_access deny RestrictedHost 
http_access allow home_network business_hours
#########################################################################
acl blockregexurl url_regex -i .facebook.com gtalk.google.com talkx.l.google.com .facebook.com .mail.gmail.com .skype.com
http_access deny blockregexurl
#########################################################################
##### Block Using Mac Address #####
#acl badmac arp E3:40:F8:01:B7:54
#http_access deny badmac
#########################################################################
#### Block Using Port of Specific IP ####
#acl block_port port 1234
#acl no_block_port_ip src 192.168.1.36
#http_access deny block_port !no_block_port_ip
#http_access allow all
#########################################################################
#####Restricting Access to specific web sites#####
# ACL blocksites 
acl blocksites dstdomain "/etc/squid/blockedsites.squid"
# Deny access to blocksites ACL
http_access deny blocksites 
#########################################################################
# ACL blockkeywords
acl blockkeywords url_regex -i "/etc/squid/blockkeywords.squid"
# Deny access to blockkeywords ACL
http_access deny blockkeywords 
#########################################################################
##### Restricting Access to specific Ipaddress #####
# ACL blockip
acl blockip src "/etc/squid/blockip.squid"
# Deny access to blockip ACL
http_access deny blockip
#########################################################################
# ACL allowip
acl allowip src "/etc/squid/allowip.squid"
http_access allow allowip
#########################################################################
##### Restricting Download size #####
#Restrict download size
#reply_body_max_size 100 MB all
#########################################################################
##### facebook Rule ######
acl fb dstdomain .facebook.com
acl officetime time M T W H F A 9:30-18:00
# Facebook Restriction
http_reply_access deny fb officetime
http_access deny CONNECT fb officetime
#########################################################################
###### Hotmail MSN Block######
acl msn url_regex messenger.hotmail.com
# Yahoo! Messenger
acl ym dstdomain .messenger.yahoo.com .psq.yahoo.com
acl ym dstdomain .us.il.yimg.com .msg.yahoo.com .pager.yahoo.com
acl ym dstdomain .rareedge.com .ytunnelpro.com .chat.yahoo.com
acl ym dstdomain .voice.yahoo.com
acl ymregex url_regex yupdater.yim ymsgr myspaceim
acl ym dstdomain .skype.com .imvu.com
# Yahoo Messenger
http_access deny ym
http_access deny ymregex
#########################################################################
##### MSN Imagine messenger Restriction#####
acl messenger_site dstdomain .imagine-msn.com/messenger
##### Google Talk Restriction #####
acl messenger_site dstdomain .talk.google.com
##### Google talk ssl Restriction #####
acl messenger_site dstdomain talkx.l.google.com:443
acl gtalk dstdomain gtalk.google.com talkx.l.google.com
##### Ebuddy Messenger Restrcition #####
acl messenger_site dstdomain .ebuddy.com
http_access deny messenger_site
#########################################################################
#### Skype Messenger Block #####
# Skype
acl numeric_IPs dstdom_regex ^(([0-9]+\.[0-9]+\.[0-9]+\.[0-9]+)|(\[([0-9af]+)?:([0-9af:]+)?:([0-9af]+)?\])):443
acl Skype_UA browser ^skype
acl validUserAgent browser \S+
# Skype Restriction
http_access deny numeric_IPS
http_access deny Skype_UA
http_access deny !validUserAgent
###########################################################################
# Hostname
visible_hostname sambafileserver
###########################################################################
# Squid normally listens to port 3128
http_port 3128 intercept
###########################################################################
```

Then add the iptables to the proxy server
Create a .sh file and save this and execute using sh

```
# vim iptables.sh
```

Enter the Script in created sh file

```
#! /bin/sh
#just for the sake of turning the networks off and on... not sure if it would work turning them back on only at the end of script ?
ifconfig eth0 down;
ifconfig eth1 down;
ifconfig lo down;
ifconfig lo up;
ifconfig eth0 up;
ifconfig eth1 up;
ifup eth0;
ifup eth1;
#I seemed to have some issues with the routing table so i thought I throw in a check up :
route add 127.0.0.1 dev lo;
route add -net 127.0.0.0/8 dev lo;
route add -net 192.168.0.0/24 dev eth1;
route add 192.168.1.0 dev eth0;
route add default gw 192.168.1.1;
# turn fowarding off while configuring iptables :
sysctl net/ipv4/ip_forward=0
iptables -F
iptables -X
iptables -P INPUT DROP
iptables -P OUTPUT DROP
iptables -P FORWARD ACCEPT
iptables -t nat -F
iptables -t nat -X
iptables -t mangle -F
iptables -t mangle -X
#And on again once the policies are set
sysctl net/ipv4/ip_forward=1
#redirect port 80 on lan card and masquerade on wan card :
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
iptables -t nat -A PREROUTING -i eth1 -p tcp --dport 80 -j REDIRECT --to-port 3128
#accept all packets in lo and protect against spoofing :
iptables -A INPUT -i lo -j ACCEPT
iptables -A OUTPUT -o lo -j ACCEPT
iptables -A INPUT -i !lo -s 127.0.0.0/8 -j DROP
iptables -A FORWARD -i !lo -s 127.0.0.0/8 -j DROP
#accept only established input but all output on WAN card
iptables -A OUTPUT -o eth0 -m state --state NEW,ESTABLISHED,RELATED -j ACCEPT
iptables -A INPUT -i eth0 -m state --state ESTABLISHED,RELATED -j ACCEPT
#just forget the invalid packets :
iptables -A OUTPUT -o eth0 -m state --state INVALID -j DROP
iptables -A INPUT -i eth0 -m state --state INVALID -j DROP
#not sure whether to put this before or after spoofing protection ?
iptables -A INPUT -i eth1 -j ACCEPT
iptables -A OUTPUT -o eth1 -j ACCEPT
#against spoofing on LAN card input :
iptables -A INPUT -i !eth1 -s 192.168.0.0/24 -j DROP
iptables -A FORWARD -i !eth1 -s 192.168.0.0/24 -j DROP

```

Then Execute the the script using 

```
# sh iptables.sh

```

To see the iptables use command

```
# iptables -L
```

Save the iptables using 

```
# iptables-save > /root/iptables_save.ip
```

If not make the iptables Script to execute after every reboot by adding it in rc file

```
# vim /etc/rc.local

sh /root/iptables.sh
```

Then Change the IP for Client machines with rage of 192.168.0.1, gateway to Proxy IPaddress

eg:
```
        address 192.168.0.99
        network 192.168.0.0
        netmask 255.255.255.0
        broadcast 192.168.0.255
        gateway 192.168.0.15
```

That's it..
