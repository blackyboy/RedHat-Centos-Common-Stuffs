#### How to Install web analytics Piwik in Centos 6.5 Final.

* Piwik is a open source web analytics software that gives you valuable insights of website’s visitors, This works same as the Google Analytics works. It Totally free, No Data Storage limits are there, Its have Community Supports, Its Available in mobile too, We can Access it from any were over the internet, The Dashboard gives us many features Such as Visitors Location, City, Browers Used, IP Address, Operating systems Used, Its a Real time Analytics.

* We can add this to Some of the CMS such as Drupal, Joomla, Wordpress etc..

* Download the piwik from Following URL

http://piwik.org/download/

* Operating system What Im Using is:

[root@monitoring ~]# lsb_release -a
Distributor ID: CentOS
Description:    CentOS release 6.5 (Final)
Release:        6.5
Codename:       Final

* Login to the Centos Machine which we need to install the analytics

```
# ssh root@192.168.1.254
```


* Navigate to the Default Document root of Apache

```
# cd /var/www/
```

* Create a Directory and navigate to the created Directory

```
# mkdir analytics && cd analytics

# pwd
/var/www/analytics
```

* Install the Dependencies Packages if we have not install before for piwik installation
  Note: The package Requierd mysqli and mbstring extensions


```
# yum install php-pdo php-mysqli -y

# yum install php-mbstring -y
```

* Download the package from 

```
# wget http://builds.piwik.org/piwik-latest.zip
```

* Unzip and remove the zip file if we Don't need in future


```
# unzip piwik-latest.zip

# rm -rf piwik-latest.zip
```

* Move the Extracted file from piwik to analytics Directory

```
# mv piwik/* .
```

* Change the Permission for the config Directory to get change the Changes while Done in the Installation time.


```
# chmod a+w config/
```

* Then We need to Create the Database and User for the piwik 
  Login to MySQL Using


``` 
# mysql -u root -p
```

* List the Database Already have

```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| dbopenfire         |
| mysql              |
+--------------------+
3 rows in set (0.03 sec)
```

* Create a Database PIWIKI and Create a User PIWIKIUSER and Create a Password for the User and grant Privileges to the user


```
mysql> CREATE DATABASE PIWIKI;
Query OK, 1 row affected (0.00 sec)

mysql> CREATE USER PIWIKIUSER@localhost;
Query OK, 0 rows affected (0.02 sec)

mysql> SET PASSWORD FOR PIWIKIUSER@localhost= PASSWORD("admin123$");
Query OK, 0 rows affected (0.00 sec)

mysql> GRANT ALL PRIVILEGES ON PIWIKI.* TO PIWIKIUSER@localhost IDENTIFIED BY 'admin123$';
Query OK, 0 rows affected (0.02 sec)

mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec)
```


* List the Database and Confirm the Creation 


```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| PIWIKI             |
| dbopenfire         |
| mysql              |
+--------------------+
4 rows in set (0.00 sec)
```

* Logout using \q

Then Navigate to the 192.168.1.15/analytics in any web Browser for future Installtion Step by Step.
