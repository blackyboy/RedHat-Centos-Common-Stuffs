How to Backup the Remote Linux Servers or Systems Using Rsnapshot 


For this we have to Setup a non password login for Root 

Only the root can perform a full backup cos only root have the administrative Privilage to access all files what ever we need to backup 

1. Setup a KeyBased Authentication 

```

#ssh-keygen

```

Create a New Key and Copy to the remote machine Using Command 

```

#ssh-copy-id -i ~/.ssh/id_rsa.pub 

```

To remote Host using Command 

```

# ssh-copy-id -i ~/.ssh/id_rsa.pub sysadmin@192.168.1.77

```

It will ask for the Password for the machine 192.168.1.77 , give the password to Authenticate 

Install the utility Rsnapshot 

```

# apt-get install rsnapshot 


```

Edit the configuration file of the Rsnapshot 

Note : Here in Configuration file Never Use the Spacebar key only u have to use the TAB Key if u need to give any spaces 

```

# vim /etc/rsnapshot.conf

```

In the Line no:27

Change the Directory if u need were to save the Backup by Default  

The Current Backup folder is under 

```

#snapshot_root   /var/cache/rsnapshot/   


```

If u need to change this default location to some were / as folder named backups

```

#snapshot_root	/backups


```

Then Enable the Line no:57 

```

# cmd_ssh /usr/bin/ssh


```

>Note : If this line Enabled only we can took backup over ssh if not we can't took backup over ssh .


If u need to change the time when need to backup 
Look at the line no:97,98,99,100

```

# retain	hourly	6

```

If You Need to backup the Localhosts directory Such as /home/, /etc/, /usr/local
uncomment line no:230,231,232

If u Don't want to took backup those Directories Comment the line with # 

Then if we need to took backup from 192.168.1.77 machine to my machine 192.168.1.99 set the command as below the Example in line no:241


```

backup	root@192.168.1.77:/etc/	/backup


```

This Command will backup the Directory /etc from 192.168.1.77 to /backup in 192.168.1.99

Save the configuration file using

```

# wq!


```

Test the rsnapshot configuration

```

# rsnapshot configtest 


```

It want to give u back a result as syntax ok

if so the test was sucess 

To know the location of rsnapshot where is use command 

```

# whereis rsnapshot 

```

If we need to backup the remote system 192.168.1.77 by mean time 

Use command 

```

# /usr/bin/rsnapshot/hourly 


```

For Automate backup using cronjob, Setup a Cronjob for rsnapshot 


This will create a cron job for current user 


```

#crontab -e 

```


In Cron job we need to define the entry by how it want to backup by hourly or by daily 


```

0	5	*	*	* /usr/bin/rsnapshot hourly 


```

this will backup hourly 


