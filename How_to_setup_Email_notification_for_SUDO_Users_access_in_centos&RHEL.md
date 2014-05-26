How to setup Email notification for SUDO Users access

### **Edit the sudoer file using 

```
# visudo
```

Enabled the sudo user and enter the following entry in bottom of the file


```
Defaults        mailto = "alertforxxxxxx@gmail.com"
Defaults        mailfrom = "root@xxxxxxx.com"
Defaults        mail_badpass
Defaults        mail_always
Defaults	      mail_no_user
Defaults        mailsub = "*** Command run via sudo on %h ***"
Defaults        badpass_message = "Please Provide Correct Password"
Defaults        !lecture,tty_tickets,!fqdn,!syslog
Defaults        logfile=/var/log/sudo.log
```

This will send email's while some user's use sudo command.
