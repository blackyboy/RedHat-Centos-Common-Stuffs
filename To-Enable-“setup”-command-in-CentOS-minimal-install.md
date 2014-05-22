no “setup” command found in CentOS minimal install

In Minimum Install this will not get install

If we get below Error, we need to install some packages to get work with setup command

```
[root@masterdns ~] setup
-bash: setup: command not found
```

Here we can see how to install in minimal install

```
yum install setuptool -y
yum install system-config-network* -y
yum install system-config-firewall* -y
yum install system-config-securitylevel-tui -y
yum install system-config-keyboard -y
yum install ntsysv -y
```

Now we can use the setup utility, That's it.
