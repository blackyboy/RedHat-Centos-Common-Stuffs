

On Linux, you can setup disk quota using one of the following methods:


1. File system base disk quota allocation
2. User or group based disk quota allocation


On the user or group based quota, following are three important factors to consider:


1. Hard limit – For example, if you specify 2GB as hard limit, user will not be able to create new files after 2GB
2. Soft limit – For example, if you specify 1GB as soft limit, user will get a warning message “disk quota exceeded”, once they reach 1GB limit. But, they’ll still be able to create new files until they reach the hard limit
3. Grace Period – For example, if you specify 10 days as a grace period, after user reach their hard limit, they would be allowed additional 10 days to create new files. In that time period, they should try to get back to the quota limit.


1. Enable quota check on filesystem

First, you should specify which filesystem are allowed for quota check.

Modify the /etc/fstab, and add the keyword usrquota and grpquota to the corresponding filesystem that you would like to monitor.

The following example indicates that both user and group quota check is enabled on /home filesystem

```

# cat /etc/fstab
LABEL=/home    /home   ext2   defaults,usrquota,grpquota  1 2

```

Reboot the server after the above change.

***

2. Initial quota check on Linux filesystem using quotacheck

Once you’ve enabled disk quota check on the filesystem, collect all quota information initially as shown below.

```

# quotacheck -avug
quotacheck: Scanning /dev/sda3 [/home] done
quotacheck: Checked 5182 directories and 31566 files
quotacheck: Old file not found.
quotacheck: Old file not found.

```

In the above command:

a: Check all quota-enabled filesystem
v: Verbose mode
u: Check for user disk quota
g: Check for group disk quota

***


3. Assign disk quota to a user using edquota command

Use the edquota command as shown below, to edit the quota information for a specific user.

For example, to change the disk quota for user ‘ramesh’, use edquota command, which will open the soft, hard limit values in an editor as shown below.

```

# edquota babinlonston

Disk quotas for user ramesh (uid 500):
  Filesystem           blocks       soft       hard     inodes     soft     hard
  /dev/sda3           1419352          0          0       1686        0        0


```

Once the edquota command opens the quota settings for the specific user in a editor, you can set the following limits:

soft and hard limit for disk quota size for the particular user.
soft and hard limit for the total number of inodes that are allowed for the particular user.


***


4. Report the disk quota usage for users and group using repquota

Use the repquota command as shown below to report the disk quota usage for the users and groups.


```


# repquota /home

```


Report for user quotas on device /dev/sda3
Block grace time: 7days; Inode grace time: 7days

```
                        Block limits                File limits
User            used    soft    hard  grace    used  soft  hard  grace
----------------------------------------------------------------------
root      --  566488       0       0           5401     0     0
nobody    --    1448       0       0             30     0     0
ramesh    -- 1419352       0       0           1686     0     0
john      --   26604       0       0            172     0     0

```


***

5. Add quotacheck to daily cron job

Add the quotacheck to the daily cron job. Create a quotacheck file as shown below under the /etc/cron.daily directory, that will run the quotacheck command everyday. This will send the output of the quotacheck command to root email address.


```


# cat /etc/cron.daily/quotacheck
quotacheck -avug

```
