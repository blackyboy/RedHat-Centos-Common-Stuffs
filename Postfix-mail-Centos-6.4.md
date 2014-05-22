Here we going to see about basic SMTP Configuration 
Here I'm using Postfix to install and configure to send mail from Centos server to any other Server's


Distribution I'm using is Centos 6.4

```
[root@Centosserver ~]# cat /etc/redhat-release 
CentOS release 6.4 (Final)
```
Scenario 

```
Pacakge		--->	postfix
script		--->	/etc/init.d/postfix
Config File	--->	/etc/postfix/main.cf
Config File	--->	/etc/postfix/master.cf
Port No		--->	25
Queue Directory	--->	/var/spool/postfix
```

Step 1:

Install the postfix Pacakge 

```
# yum install postfix
```

Step 2:

Edit the Configuration file of Postfix using vim editor

```
# vim /etc/postfix/main.cf

```
Uncomment and Replace the Hostname in Line 75

```
myhostname = Centosserver.example.com
```

In Line 98 Uncomment and Don't need to do changes there.

```
myorigin = $myhostname
```

In Line 113, Uncomment.
UnCommenting this line will make it to receive mail from all networks

```
inet_interfaces = all
```

In Line 116 Comment with # 
Here we Commenting With # Because we need to Receive mails from all hosts, If this not uncommented, Only 
localhost mails can recive

```
#inet_interfaces = localhost
```

In Line 264, Uncomment and add the Network Range were Our Server Located 

```
mynetworks = 168.100.189.0/28, 127.0.0.0/8, 192.168.0.0/24

```
Here i Have added 192.168.0.0/24 for tutorial Puropose 

In Line 426, uncomment 

```
mail_spool_directory = /var/spool/mail

```
This is the mail Qeue Directory


Check the Configuration Once more:

```
myhostname = Centosserver.example.com
myorigin = $myhostname
inet_interfaces = all
#inet_interfaces = localhost
mynetworks = 168.100.189.0/28, 127.0.0.0/8, 192.168.0.0/24
mail_spool_directory = /var/spool/mail
```

Step 3:

Restart the Postfix Server 

```
# /etc/init.d/postfix restart
```

step 5:

Add a user to check wether mail is working 

```
[root@Centosserver ~]# useradd user1
[root@Centosserver ~]# passwd user1
```

step 6:

Send a mail from Root user to user1

To send a mail 
Just type as mail and then address were we need to send 
Type the Subject and Content and for sending the mail just put a .(dot) and press enter, mail will be send 

```
eg :

[root@Centosserver ~]# mail user1@Centosserver.example.com
Subject: Linux Mental mail setup                           
Hi this is a test mail from Centosserver root user to user1 
.
EOT
```

Step 7:

```
su - user1

```
To check mail just type as mail, if so You will get like this below 

Output:

```
[root@Centosserver ~]# su - user1
[user1@Centosserver ~]$ mail
Heirloom Mail version 12.4 7/29/08.  Type ? for help.
"/var/spool/mail/user1": 1 message 1 new
>N  1 root                  Thu Dec  5 18:27  18/687   "Linux Mental mail setup"
& 
```

Choose 1 and press enter to read the mail

```
output:
From root@Centosserver.example.com  Thu Dec  5 18:27:50 2013
Return-Path: <root@Centosserver.example.com>
X-Original-To: user1@Centosserver.example.com
Delivered-To: user1@Centosserver.example.com
Date: Thu, 05 Dec 2013 18:27:50 +0530
To: user1@Centosserver.example.com
Subject: Linux Mental mail setup
User-Agent: Heirloom mailx 12.4 7/29/08
Content-Type: text/plain; charset=us-ascii
From: root@Centosserver.example.com (root)
Status: R

Hi this is a test mail from linux mental root user to user1

& 
```

That's it...

