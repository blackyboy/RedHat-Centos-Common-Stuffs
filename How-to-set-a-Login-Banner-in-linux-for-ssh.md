How to set a Login Banner in linux for ssh 


First we need to create a file in /etc/ or in any were here im going to create inside /etc/


1. First create a file named issue 


```

#sudo vim /etc/issue 

```


![1](https://github.com/babinlonston/All-Common-Linux-Stuffs/raw/master/ssh%20Banner/Selection_001.png)



2. Then paste your copyright content as u wish ..
Here im using following content 



```
NOTICE TO USERS


This computer system is the private property of its owner, whether
individual, corporate or government.  It is for authorized use only.
Users (authorized or unauthorized) have no explicit or implicit
expectation of privacy.

Any or all uses of this system and all files on this system may be
intercepted, monitored, recorded, copied, audited, inspected, and
disclosed to your employer, to authorized site, government, and law
enforcement personnel, as well as authorized officials of government
agencies, both domestic and foreign.

By using this system, the user consents to such interception, monitoring,
recording, copying, auditing, inspection, and disclosure at the
discretion of such personnel or officials.  Unauthorized or improper use
of this system may result in civil and criminal penalties and
administrative or disciplinary action, as appropriate. By continuing to
use this system you indicate your awareness of and consent to these terms
and conditions of use. LOG OFF IMMEDIATELY if you do not agree to the
conditions stated in this warning.

****************************************************************************
```


3. Then Edit the file sshd_config 



```

# sudo vim/etc/ssh/sshd_config

```


![2](https://github.com/babinlonston/All-Common-Linux-Stuffs/raw/master/ssh%20Banner/Selection_002.png)



4. Then Add a line at bottom 



```

#banner /etc/issue


```



![3](https://github.com/babinlonston/All-Common-Linux-Stuffs/raw/master/ssh%20Banner/Selection_003.png)



save the file and exit 


5. Then restart the ssh service 


```

#sudo /etc/init.d/ssh restart


```


![4](https://github.com/babinlonston/All-Common-Linux-Stuffs/raw/master/ssh%20Banner/Selection_004.png)




Then Logout and login again to see the message what u have been saved in the issue file ...
That's it ..

 


![5](https://github.com/babinlonston/All-Common-Linux-Stuffs/raw/master/ssh%20Banner/Selection_005.png)




