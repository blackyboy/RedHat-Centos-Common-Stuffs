```
scp: command not found
```
If we face scp: command not found with Centos minimal Install 
we need to install the package **openssh-clients**

If the Package was not Present we can able to copy files over network using scp and rsync

```
# yum install openssh-clients -y
```

This will fix the issue to copy file between remote hosts