2 Methods To Change TimeZone in Linux

For this example, assume that your current timezone is UTC as shown below. You would like to change this to Asia Time.


```

#date

Fri Jul 19 18:16:21 IST 2013

```


On some distributions (for example, CentOS), the timezone is controlled by /etc/localtime file.

Delete the current localtime file under /etc/ directory


```

# cd /etc

#rm localtime

```

All Asia timezones are located under under the /usr/share/zoneinfo/Asia directory as shown below.


```

# ls /usr/share/zoneinfo/Asia/
India	Pakistan	Nepal	Kolkata	Srilanka

```
	
Note: For other country timezones, browse the /usr/share/zoneinfo directory



Link the Asia file from the above Asia directory to the /etc/localtime directory as shown below.


```

# date

Fri Jul 19 18:16:21 IST 2013


```

Method 2: Change TimeZone Using /etc/timezone File


On some distributions (for example, Ubuntu), the timezone is controlled by /etc/timezone file.

For example, your current timezone might be US Eastern time (New York) as shown below.


```

# cat /etc/timezone
America/New_York


```
To change this to US Pacific time (Los Angeles), modify the /etc/timezone file as shown below.


```

# vim /etc/timezone
America/Los_Angeles

```

Also, set the timezone from the command line using the TZ variable.


```

# export TZ=America/Los_Angeles


```


