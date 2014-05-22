How to install and use uTorrent in Ubuntu 12.04, 13.04 in Wen Interface 

Move to download Directory 

```

# cd /home/babinlonstonadmin/Downloads 

```

Download the utorrent for linux from Utorrent.com 

```

#wget http://download.utorrent.com/linux/utorrent-server-3.0-25053.tar.gz


```
 OR             	
Pull from GitHub 

```

#git pull https://github.com/babinlonston/All-Common-And-RHEL-Linux-Stuffs/blob/master/utorrent-server-3.0-ubuntu-10.10-27079.tar.gz

```

Next, run the commands below to extract uTorrent files to the /opt directory.

```

# sudo tar xvzf utorrent-server-3.0-25053.tar.gz -C /opt/

```

Then run the commands below to change the permission on uTorrent-server folder.

```

# sudo chmod -R 777 /opt/utorrent-server-v3_0/

```

Next, run the commands below to link uTorrent server to the /user/bin directory.

```

# sudo ln -s /opt/utorrent-server-v3_0/utserver /usr/bin/utserver

```

Finally, run the commands below to start uTorrent.

```

#utserver -settingspath /opt/utorrent-server-v3_0/

```

If you get an error about libssl.so package missing, run the commands below to install it, then try starting it again.


```

# sudo apt-get install libssl0.9.8:i386


```

Now that uTorrent server is started, open your web browser (Firefox) and type the address below.

```

http://localhost:8080/gui/


```


The username is admin and leave the password field empty. 


![This is the Web interface of utorrent in linux](https://github.com/babinlonston/All-Common-And-RHEL-Linux-Stuffs/blob/master/D:%200.0%20kB-s%20U:%200.0%20kB-s%20-%20Google%20Chrome_001.jpg)


Enjoy Using utorrent ...




