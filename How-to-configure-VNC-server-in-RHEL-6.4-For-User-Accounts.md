VNC Packages 

1. Tigervnc - server
2. Open ssh 

Configuration File 

```

# /etc/sysconfig/vncservers

```

Port Number used by VNC server

Port 5900 , 5901 , 5902

Install VNC using Command Line 

```

# yum install tigervnc* -y 

```

To edit the Config file 

```

# vim /etc/sysconfig/vncserver

```

Remove the # in Last 2 Lines of the configuration file 

Example : 

```

#VNCSERVERS="2:babinlonston"
#VNCSERVERARGS[2]="-geometry 800x600 -nolisten tcp -localhost"

```

Uncomment the # to Activated 

Save the File using command 

```

#wq!

```

Switch the user to babinlonston using 

```

# su - babinlonston

```

Create a Vnc password as 

```

#vncpasswd 
Newpass      : **********
verifypasswd : **********

```
 
Restart the service using command 

```

# /etc/init.d/vncserver restart 

```

Set the VNC server to run in 5 level 

```

# chkconfig vncserver on 

```

Then Connect to the user babinlonston using the command 

```

# vncviewer -via babinlonston@server.example.com localhost:2 

```

That's it we can view the user babinlonston's desktop in vnc 
