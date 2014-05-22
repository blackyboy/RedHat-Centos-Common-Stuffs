Add this in virtualhost of 80

Edit the file 

```
vim /etc/httpd/conf/httpd.conf
```

And add this in virtualhost 

```
RewriteEngine On
RewriteCond %{HTTP:X-Forwarded-Proto} !=https
RewriteCond %{REQUEST_URI} !^/health_check
RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI} [L]
```