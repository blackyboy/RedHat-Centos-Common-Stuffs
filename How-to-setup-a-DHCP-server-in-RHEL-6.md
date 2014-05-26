How to setup a DHCP server in RHEL

```
IP Address 	:	192.168.0.200
Hostname	:	dhcpserver.linuxmental.com
```

Operating System:

```
# lsb_release -a

LSB Version:	:core-4.0-amd64:core-4.0-noarch:graphics-4.0-amd64:graphics-4.0-noarch:printing-4.0-amd64:printing-4.0-noarch
Distributor ID:	RedHatEnterpriseServer
Description:	Red Hat Enterprise Linux Server release 6.2 (Santiago)
Release:	6.2
Codename:	Santiago
```

Here we going to install the dhpcd package

```
# yum install dhcp* -y
```

Then copy the configuration file from below location to etc/dhcp

```
# cp /usr/share/doc/dhcp*/dhcpd.conf.sample /etc/dhcp/
```

Rename the file to dhcpd.conf

```
# mv /etc/dhcp/dhcpd.conf.sample /etc/dhcp/dhcpd.conf
```

Edit the Config file to change the need configuration 

```
# vim /etc/dhcp/dhcpd.conf
```
In Line number 7,8 Change the Domain name and Name servers

```
option domain-name "linuxmental.local";
option domain-name-servers masterdns.linuxmental.local, slavedns.linuxmental.local;
```

In Line number 10, 11 check for the lease time and max lease

```
default-lease-time 600;
max-lease-time 7200;
```

Enabled the line 18 

```
authoritative;
```

In Line 22 Change the local7 to local6 for logs

```
log-facility local6;
```

Then we need to edit the file under 

```
# vim /etc/rsyslog.conf
```

Add a New line under local7 in line number 58

```
local6.*                        /var/log/dhcpd.log
```
Save and Exit the rsyslog.conf and go back to dhcpd.conf

In Line 32 Add the subnet of network we need to enabled DHCP and Add the Range of IP, Comment the Router 

```
subnet 192.168.0.0 netmask 255.255.255.0 {
  range 192.168.0.205 192.168.0.210;
# option routers rtr-239-0-1.example.org, rtr-239-0-2.example.org;
}
```

Add the Fixed IP and Hostname for the Client machine in Fixed IP sections if you already have a valid DNS server 

```
host node1 {
  hardware ethernet 52:54:00:17:3B:C7;
  fixed-address node1.linuxmental.local;
}

host node2 {
  hardware ethernet 52:54:00:1B:86:E0;
  fixed-address node2.linuxmental.local;
}

host node3 {
  hardware ethernet 52:54:00:DE:73:FD;
  fixed-address node3.linuxmental.local;
}
```

Then Start the DHCP Service 

```
# service dhcpd restart
```
Make the Service Runnable in Multi Level's

```
# chkconfig dhcpd on
# chkconfig --list dhcpd
```

That's it we have Setup-ed a DHCP Server in RHEL Server.

