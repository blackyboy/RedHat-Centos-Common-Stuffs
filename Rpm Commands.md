RPM = Red Hat Package manager or RPM Package Manager
Rpm is a Installation package which Used to get install in Redhat distributions
Mostly RPM were used in rhel systems, RPM can be build with sign verified, And same used in fedora, centos operating systems too.

Modes Of RPM Commands

``` 
Install	= i	#i Used to Install the package
Erase	= e	#e used to remove the package
Upgrade	= U	#U Used to Upgrade the Package
Verify	= v	#v Used to verify the pcakge 
Query	= q	#q Used to Query about package
Hash	= h	#h Print 50 hash marks while package archive is unpacked 

```
1. To Query all packages, Means Searching the Installed RPM Packages 

```
[root@server ~]# rpm -q -a


         OR

[root@server ~]# rpm -qa
```

2. To Get the Installed Specified Package  

```
[root@server ~]# rpm -qa httpd
httpd-2.2.15-15.el6.x86_64

          OR

[root@server ~]# rpm -q -a httpd
httpd-2.2.15-15.el6.x86_64
```

3. To Show the installed package Information in RPM 

```
[root@server ~]# rpm -q -i httpd


Output:

Name        : httpd                        Relocations: (not relocatable)
Version     : 2.2.15                            Vendor: Red Hat, Inc.
Release     : 15.el6                        Build Date: Thursday 06 October 2011 08:37:25 PM IST
Install Date: Monday 18 November 2013 01:47:26 AM IST      Build Host: x86-005.build.bos.redhat.com
Group       : System Environment/Daemons    Source RPM: httpd-2.2.15-15.el6.src.rpm
Size        : 3061330                          License: ASL 2.0
Signature   : RSA/8, Tuesday 08 November 2011 09:20:50 PM IST, Key ID 199e2f91fd431d51
Packager    : Red Hat, Inc. <http://bugzilla.redhat.com/bugzilla>
URL         : http://httpd.apache.org/
Summary     : Apache HTTP Server
Description :
The Apache HTTP Server is a powerful, efficient, and extensible
web server.
```

4. To Show were ever the files Created While specific package get installed.

```
[root@server ~]# rpm -q -l httpd

Output:
[root@server ~]# rpm -q -l httpd
/etc/httpd
/etc/httpd/conf
/etc/httpd/conf.d
/etc/httpd/conf.d/README
/etc/httpd/conf.d/welcome.conf
/etc/httpd/conf/httpd.conf
/etc/httpd/conf/magic
```

5. To Show the Only Configuration files 

```
[root@server ~]# rpm -q -c httpd

             OR 

[root@server ~]# rpm -q --configfiles httpd

Output:
[root@server ~]# rpm -q -c httpd
/etc/httpd/conf.d/welcome.conf
/etc/httpd/conf/httpd.conf
/etc/httpd/conf/magic
/etc/logrotate.d/httpd
/etc/sysconfig/httpd
/var/www/error/HTTP_BAD_GATEWAY.html.var
```

6. To Query the Installed Docs releated to a Specific Pacakge 

```
[root@server ~]# rpm -qd httpd

           OR

[root@server ~]# rpm -q --docfiles httpd

Output:
[root@server ~]# rpm -qd httpd
/usr/share/doc/httpd-2.2.15/ABOUT_APACHE
/usr/share/doc/httpd-2.2.15/CHANGES
/usr/share/doc/httpd-2.2.15/LICENSE
/usr/share/doc/httpd-2.2.15/NOTICE
/usr/share/doc/httpd-2.2.15/README
/usr/share/doc/httpd-2.2.15/VERSIONING
```

7. To see the actions of Installation of Particular package. 

```
[root@server ~]# rpm -q --scripts httpd

Output:
[root@server ~]# rpm -q --scripts httpd
preinstall scriptlet (using /bin/sh):
# Add the "apache" user
getent group apache >/dev/null || groupadd -g 48 -r apache
getent passwd apache >/dev/null || \
  useradd -r -u 48 -g apache -s /sbin/nologin \
    -d /var/www -c "Apache" apache
exit 0
```
8. To Check RPM Signature for a Package

[root@server ~]# rpm --checksig epel-release-6-8.noarch.rpm 

Output:
[root@server ~]# rpm --checksig epel-release-6-8.noarch.rpm 
epel-release-6-8.noarch.rpm: RSA sha1 ((MD5) PGP) md5 NOT OK (MISSING KEYS: (MD5) PGP#0608b895)
```

9. To Install a Pacakge

```
[root@server ~]# rpm -ivh epel-release-6-8.noarch.rpm 

Output:
[root@server ~]# rpm -ivh epel-release-6-8.noarch.rpm 
warning: epel-release-6-8.noarch.rpm: Header V3 RSA/SHA256 Signature, key ID 0608b895: NOKEY
Preparing...                ########################################### [100%]
	package epel-release-6-8.noarch is already installed
```

10. To Display the recently Installed Pacakges

```
[root@server ~]# rpm -qa --last | less

Ouput:
epel-release-6-8.noarch                       Thursday 05 December 2013 08:11:11 AM UTC
mysql-server-5.1.71-1.el6.x86_64              Wednesday 04 December 2013 06:49:16 AM UTC
perl-DBI-1.609-4.el6.x86_64                   Wednesday 04 December 2013 06:49:15 AM UTC
perl-DBD-MySQL-4.013-3.el6.x86_64             Wednesday 04 December 2013 06:49:15 AM UTC
zlib-1.2.3-29.el6.i686                        Wednesday 04 December 2013 06:49:06 AM UTC
ncurses-libs-5.7-3.20090208.el6.i686          Wednesday 04 December 2013 06:49:06 AM UTC
bzip2-libs-1.0.5-7.el6_0.i686                 Wednesday 04 December 2013 06:49:06 AM UTC
```

11. To Upgrade a Package Use command

```
[root@server ~]# rpm -Uvh epel-release-6-8.noarch.rpm 
```

12. Query a File and get the info about file belongs to Which package

```
[root@server ~]# rpm -qf /usr/bin/rpmdb 

Output:
[root@server ~]# rpm -qf /usr/bin/rpmdb 
rpm-4.8.0-32.el6.x86_64
```

13. To List all Files Inside a RPM Package 

```
#[root@server ~]# rpm -qlp epel-release-6-8.noarch.rpm

```
Ouput:
```
[root@server ~]# rpm -qlp epel-release-6-8.noarch.rpm 
warning: epel-release-6-8.noarch.rpm: Header V3 RSA/SHA256 Signature, key ID 0608b895: NOKEY
/etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-6
/etc/rpm/macros.ghc-srpm
/etc/yum.repos.d/epel-testing.repo
/etc/yum.repos.d/epel.repo
/usr/share/doc/epel-release-6
/usr/share/doc/epel-release-6/GPL
```

14. Importing RPM GPG Key to Verify the Packages

```
[root@server ~]# rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6 
 
```

15. To List all Imported keys Use command

```
[root@server ~]# rpm -qa gpg-pubkey*

Output:
[root@server ~]# rpm -qa gpg-pubkey*
gpg-pubkey-c105b9de-4e0fd3a3
```

16. To Remove a Package

```
[root@server ~]# rpm -ev epel-release-6-8.noarch

```

17. To verify all pacakges

```
[root@server lib]# rpm -Va

Output:
[root@server ~]# rpm -Va
S.5....T.  c /etc/httpd/conf/httpd.conf
..5....T.  c /usr/lib64/security/classpath.security
S.5....T.  c /etc/postfix/main.cf
```

18. To know More About RPM commands 

```
man rpm
```
