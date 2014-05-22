Here we going to see about yum commands 
what is yum ? yum is a package installer; yum Actually called as yellow dog update manager 
yum is an interactive, rpm based, package manager. It can automatically perform system updates, including dependency analysis and obsolete pro-cessing  based  on "repository" metadata. It can also perform installa-tion of new packages

1. To install a package Using yum command, here I'm installing nmap a small Network scanner tool

```
[root@server ~]# yum install nmap
```
2. To list the Installed and Available Packages 

```
[root@server ~]# yum list

```

There are many packages and we can't see in one fixed page, so for viewing all package we need to use the less command 

```
[root@server ~]# yum list | less

```

This will give us a scroll view 

3. If we need to see the Source Pacakges Which all Available for installation
This below command will give us all packages are available in Source were we going to get from. 

```
[root@server ~]# yum list available 
```

4 . To view the Installed packages, this will show the packages which all installed in our Server/system

```
[root@server ~]# yum list installed

```
5 . Yum Search Command :

Here I Need to Install iSCSI pacakge for client, I don't Remember the package name But i know it's start with iSCSI, For installing the package we need full name, So i need to search the correct package in this Situation we can use search command

```
# [root@server ~]# yum search iSCSI

Loaded plugins: product-id, refresh-packagekit, security, subscription-manager
Updating certificate-based repositories.
===================================N/S Matched: iSCSI===================================
iscsi-initiator-utils.x86_64 : iSCSI daemon and utility programs

  Name and summary matches only, use "search all" for everything.
```

here i Have Used iSCSI, But it will search and give us the package and its related.


6. If we need to see any Particular Pacakge Information.
   Here I'm Gathering info for iSCSI client package 

```
# yum info iscsi-initiator-utils.x86_64
```
Output :
```
[root@server ~]# yum info iscsi-initiator-utils.x86_64
Loaded plugins: product-id, refresh-packagekit, security, subscription-manager
Updating certificate-based repositories.
Installed Packages
Name        : iscsi-initiator-utils
Arch        : x86_64
Version     : 6.2.0.872
Release     : 34.el6
Size        : 2.2 M
Repo        : installed
From repo   : anaconda-RedHatEnterpriseLinux-201111171049.x86_64
Summary     : iSCSI daemon and utility programs
URL         : http://www.open-iscsi.org
License     : GPLv2+
Description : The iscsi package provides the server daemon for the iSCSI protocol,
            : as well as the utility programs used to manage it. iSCSI is a protocol
            : for distributed disk access using SCSI commands sent over Internet
            : Protocol networks.
```
7. If we need to install a pacakge without asking a comfirmation y/n Option, Use command -y

```
[root@server ~]# yum install iscsi-initiator-utils.x86_64 -y

```

8. To Remove a Package, Here I'm removing nmap Utility

```
[root@server ~]# yum remove nmap

            or 

[root@server ~]# yum erase nmap

```
Output:

```
[root@server ~]# yum remove nmap
Loaded plugins: product-id, refresh-packagekit, security, subscription-manager
Updating certificate-based repositories.
Setting up Remove Process
Resolving Dependencies
--> Running transaction check
---> Package nmap.x86_64 2:5.21-4.el6 will be erased
--> Finished Dependency Resolution

Dependencies Resolved

============================================================================================================================================================================
 Package                              Arch                                   Version                                         Repository                                Size
============================================================================================================================================================================
Removing:
 nmap                                 x86_64                                 2:5.21-4.el6                                    @package                                 7.3 M

Transaction Summary
============================================================================================================================================================================
Remove        1 Package(s)

Installed size: 7.3 M
Is this ok [y/N]: 

```

9. To Update the Server/system use command 

```
[root@server ~]# yum update

```

10. To list the yum packages Group 

```
[root@server ~]# yum grouplist

```

output :

```
Loaded plugins: product-id, refresh-packagekit, security, subscription-manager
Updating certificate-based repositories.
Setting up Group Process
Installed Groups:
   Additional Development
   Base
   Client management tools
   Debugging Tools
   Desktop
   Desktop Debugging and Performance Tools
   Desktop Platform
   Dial-up Networking Support
   Directory Client
   E-mail server
   FCoE Storage Client
   FTP server
   Fonts
```

11. To View the Pacakages Which all Available inside the Group

```
[root@server ~]# yum groupinfo Desktop

```

Output: 

```
[root@server ~]# yum groupinfo Desktop
Loaded plugins: product-id, refresh-packagekit, security, subscription-manager
Updating certificate-based repositories.
Setting up Group Process

Group: Desktop
 Description: A minimal desktop that can also be used as a thin client.
 Mandatory Packages:
   NetworkManager
   NetworkManager-gnome
   alsa-plugins-pulseaudio
   at-spi
   control-center
   dbus
   gdm
```

12. To install a Group of pacakges 

```
[root@server ~]# yum groupinstall directory-client

[root@server ~]# yum groupinstall directory-client
Loaded plugins: product-id, refresh-packagekit, security, subscription-manager
Updating certificate-based repositories.
Setting up Group Process
Package 3:ypbind-1.20.4-29.el6.x86_64 already installed and latest version
Package ipa-client-2.1.3-9.el6.x86_64 already installed and latest version
Package oddjob-mkhomedir-0.30-5.el6.x86_64 already installed and latest version
Package sssd-1.5.1-66.el6.x86_64 already installed and latest version
```

Here i have already installed, so what it showing as already installed 

13. If we need to Update a Group of Pacakges we can update using 

```
# yum groupupdate directory-client

```

14. To Remove the Group of package use command 

```
[root@server ~]# yum groupremove directory-client

                    Or 

[root@server ~]# yum grouperase directory-client 

```
Output: 

```
[root@server ~]# yum grouperase directory-client
Loaded plugins: product-id, refresh-packagekit, security, subscription-manager
Updating certificate-based repositories.
Setting up Group Process
Resolving Dependencies
--> Running transaction check
---> Package certmonger.x86_64 0:0.50-3.el6 will be erased
---> Package ipa-client.x86_64 0:2.1.3-9.el6 will be erased
---> Package krb5-workstation.x86_64 0:1.9-22.el6 will be erased
---> Package oddjob-mkhomedir.x86_64 0:0.30-5.el6 will be erased
---> Package pam_krb5.x86_64 0:2.3.11-9.el6 will be erased
---> Package sssd.x86_64 0:1.5.1-66.el6 will be erased
---> Package ypbind.x86_64 3:1.20.4-29.el6 will be erased
--> Processing Dependency: ypbind for package: yp-tools-2.9-12.el6.x86_64
--> Running transaction check
---> Package yp-tools.x86_64 0:2.9-12.el6 will be erased
--> Finished Dependency Resolution

```

15. If we need to install a Package After Updating 

```
# yum update -y && yum install nmap -y 

```
This will update first and next it will install the nmap package
This is what we call chain command Using (&&) 

16. To Display the Packages Which all not installed from RHEL Source or from Centos Source 

```
[root@media ~]# yum list extras
```
output:
```
[root@media ~]# yum list extras
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: centos.sonn.com
 * extras: centos.mirror.ndchost.com
 * updates: linux.mirrors.es.net
Extra Packages
libblkid.x86_64                                                        2.17.2-12.9.el6_4.3                                               @updates 
libcom_err.x86_64                                                      1.41.12-14.el6_4.2                                                @updates 
libdrm.x86_64                                                          2.4.39-1.el6                                                      @base    
libgcc.x86_64                                                          4.4.7-3.el6                                                       installed
libgcrypt.x86_64                                                       1.4.5-9.el6_2.2                                                   installed
libnl.x86_64                                                           1.1.4-1.el6_4                                                     @updates 
libss.x86_64                                                           1.41.12-14.el6_4.2                                                @updates 
libstdc++.x86_64                                                       4.4.7-3.el6                                                       installed
libuuid.x86_64                                                         2.17.2-12.9.el6_4.3                                               @updates 
```
17. To Display the Current Repository We using, Use Command 
```
[root@media ~]# yum repolist

Output:
[root@media ~]# yum repolist
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: centos.sonn.com
 * extras: centos.mirror.ndchost.com
 * updates: linux.mirrors.es.net
repo id                                                         repo name                                                                   status
base                                                            CentOS-6 - Base                                                             6,367
extras                                                          CentOS-6 - Extras                                                              13
updates                                                         CentOS-6 - Updates                                                             60
repolist: 6,440
```
18. To Display all Repository 
```
[root@media ~]# yum repolist all

Output:
[root@media ~]# yum repolist all
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: centos.sonn.com
 * extras: centos.mirror.ndchost.com
 * updates: linux.mirrors.es.net
repo id                                                       repo name                                                             status
C6.0-base                                                     CentOS-6.0 - Base                                                     disabled
C6.0-centosplus                                               CentOS-6.0 - CentOSPlus                                               disabled
C6.0-contrib                                                  CentOS-6.0 - Contrib                                                  disabled
C6.0-extras                                                   CentOS-6.0 - Extras                                                   disabled
C6.0-updates                                                  CentOS-6.0 - Updates                                                  disabled
C6.1-base                                                     CentOS-6.1 - Base                                                     disabled
C6.1-centosplus                                               CentOS-6.1 - CentOSPlus                                               disabled
C6.1-contrib                                                  CentOS-6.1 - Contrib                                                  disabled
C6.1-extras                                                   CentOS-6.1 - Extras                                                   disabled
C6.1-updates                                                  CentOS-6.1 - Updates                                                  disabled
C6.2-base                                                     CentOS-6.2 - Base                                                     disabled
C6.2-centosplus                                               CentOS-6.2 - CentOSPlus                                               disabled
C6.2-contrib                                                  CentOS-6.2 - Contrib                                                  disabled
C6.2-extras                                                   CentOS-6.2 - Extras                                                   disabled
C6.2-updates                                                  CentOS-6.2 - Updates                                                  disabled
C6.3-base                                                     CentOS-6.3 - Base                                                     disabled
C6.3-centosplus                                               CentOS-6.3 - CentOSPlus                                               disabled
C6.3-contrib                                                  CentOS-6.3 - Contrib                                                  disabled
C6.3-extras                                                   CentOS-6.3 - Extras                                                   disabled
C6.3-updates                                                  CentOS-6.3 - Updates                                                  disabled
base                                                          CentOS-6 - Base                                                       enabled: 6,367
c6-media                                                      CentOS-6 - Media                                                      disabled
centosplus                                                    CentOS-6 - Plus                                                       disabled
contrib                                                       CentOS-6 - Contrib                                                    disabled
debug                                                         CentOS-6 - Debuginfo                                                  disabled
extras                                                        CentOS-6 - Extras                                                     enabled:    13
updates                                                       CentOS-6 - Updates                                                    enabled:    60
repolist: 6,440
```
19. Running Yum Command Using Shell 
```
[root@media ~]# yum shell
```
Then Type the command we need to execute
Here I'm Getting info of nmap Package 
```
> info nmap
Loading mirror speeds from cached hostfile
 * base: centos.mirror.freedomvoice.com
 * extras: centos.mirror.ndchost.com
 * updates: linux.mirrors.es.net
Available Packages
Name        : nmap
Arch        : x86_64
Epoch       : 2
Version     : 5.51
Release     : 3.el6
Size        : 2.7 M
Repo        : base
Summary     : Network exploration tool and security scanner
URL         : http://nmap.org/
License     : GPLv2 and LGPLv2+ and GPLv2+ and BSD
Description : Nmap is a utility for network exploration or security auditing.  It supports
            : ping scanning (determine which hosts are up), many port scanning techniques
            : (determine what services the hosts are offering), and TCP/IP fingerprinting
            : (remote host operating system identification). Nmap also offers flexible target
            : and port specification, decoy scanning, determination of TCP sequence
            : predictability characteristics, reverse-identd scanning, and more. In addition
            : to the classic command-line nmap executable, the Nmap suite includes a flexible
            : data transfer, redirection, and debugging tool (netcat utility ncat), a utility
            : for comparing scan results (ndiff), and a packet generation and response analysis
            : tool (nping).
```
20. Installing Packages from Disabled Repository using enablerepo
```
# yum --enablerepo=epel install nmap
```
Output:
```
[root@media ~]# yum --enablerepo=epel install nmap
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
epel/metalink                                                                                                              |  14 kB     00:00     
 * base: centos.sonn.com
 * epel: linux.mirrors.es.net
 * extras: centos.mirror.ndchost.com
 * updates: linux.mirrors.es.net
epel                                                                                                                       | 4.2 kB     00:00     
epel/primary_db                                                                                                            | 5.7 MB     00:00     
Setting up Install Process
Resolving Dependencies
--> Running transaction check
---> Package nmap.x86_64 2:5.51-3.el6 will be installed
--> Processing Dependency: libpcap.so.1()(64bit) for package: 2:nmap-5.51-3.el6.x86_64
--> Running transaction check
---> Package libpcap.x86_64 14:1.4.0-1.20130826git2dbcaa1.el6 will be installed
--> Finished Dependency Resolution
```
21. If Packages in Local and we need to install it Use command 

Here My rpm packages are Located under /home/user/nmap.rpm

```
# yum localinstall /home/user/*.rpm
```

This will install all packages Inside the /home/user/ Directory

22. If we need to know what the Use of Specified Package 

```
[root@media ~]# yum provides nmap
```
Output:
```
[root@media ~]# yum provides nmap
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: centos.sonn.com
 * extras: centos.mirror.ndchost.com
 * updates: linux.mirrors.es.net
2:nmap-5.51-3.el6.x86_64 : Network exploration tool and security scanner
Repo        : base
Matched from:
```
23. While Installing Packages, If we need to Exclude some packages Use command
```
# yum install nmap --exclude libpcap
```
This will Exclude the Particular Which we need to exclude

24. To cleanup any cached packages in any enabled repository's cache directory.
```
#  yum clean packages
```
25. To cleanup xml metadata which chaced from repository
```
# yum clean metadata
```
26. To clear any caches from any repository 
```
# yum clean all 
```
27. To check Update using YUM
```
# yum check-update
```
28. To Generate the metadata cache
```
# yum makecache
```
29. To Downgrade a Version of Some Softwares
```
[root@media tmp]# yum downgrade pidgin
```
This will Downgrade the version of pidgin

30. To know More About Yum Command 
```
# man yum
```