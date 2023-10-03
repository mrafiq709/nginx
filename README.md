# nginx

Step 1 – Installing Nginx
--------------------------
```
For ubuntu
sudo apt update
sudo apt install nginx

For mac
brew install nginx
```
Step 2 – Adjusting the Firewall
-------------------------------
```
sudo ufw app list
```
```
Output:
-------
Available applications:
  Nginx Full
  Nginx HTTP
  Nginx HTTPS
  OpenSSH
```
If 'Nginx HTTP' not listed
```
sudo ufw allow 'Nginx HTTP'
sudo ufw status
```
```
Output:
-------
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere                  
Nginx HTTP                 ALLOW       Anywhere                  
OpenSSH (v6)               ALLOW       Anywhere (v6)             
Nginx HTTP (v6)            ALLOW       Anywhere (v6)
```
Step 3 – Checking your Web Server
---------------------------------
```
systemctl status nginx
systemctl reload nginx
systemctl restart nginx
```
For mac
```
brew services stop nginx
brew services start nginx

sudo nginx
sudo nginx -s reload
sudo nginx -s stop
```
See error Log
--------------
```
sudo tail -30 /var/log/nginx/error.log
```
For mac
```
sudo tail -n 50 /usr/local/var/log/nginx/error.log
sudo tail -n 50 /usr/local/var/log/php-fpm.log
```
Processing php file:
--------------------
```
location ~ \.php$ {
		include fastcgi_params;
                fastcgi_intercept_errors on;
	        fastcgi_index  index.php;
		# With php-fpm (or other unix sockets):
		fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
	}
```
for mac /usr/local/etc/nginx/servers/connect.test
```
    server {
        listen       8081;
        server_name  connect.test;

        root /usr/local/var/www/test;
        index index.php

        location ~ \.php$ {
            include fastcgi_params;
            fastcgi_pass 127.0.0.1:9000;  # Match the PHP-FPM listen address
            fastcgi_index index.php;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        }
    }
```
Config php-fpm for mac:
```
nano /usr/local/etc/php/8.2/php-fpm.d/www.conf

[www]
user = mdrafiqulislam
group = staff

listen = 127.0.0.1:9000
listen.owner = mdrafiqulislam
listen.group = staff
listen.allowed_clients = 127.0.0.1

pm = dynamic
pm.max_children = 5
pm.start_servers = 2
pm.min_spare_servers = 1
pm.max_spare_servers = 3
```

Error:
<a href="https://imgur.com/DeeoTZw"><img src="https://i.imgur.com/DeeoTZw.png" title="source: imgur.com" /></a><br/><br/>
Solution:
```
sudo nginx -t -c /etc/nginx/nginx.conf
```
output:
> nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
>
> nginx: configuration file /etc/nginx/nginx.conf test is successful

```
sudo fuser -k 80/tcp
sudo fuser -k 443/tcp
sudo service nginx restart
sudo service nginx status
```
<a href="https://imgur.com/lvJkgZA"><img src="https://i.imgur.com/lvJkgZA.png" title="source: imgur.com" /></a><br/><br/>

For mac
```
sudo brew services list
sudo brew services start php
sudo php-fpm -t
sudo launchctl unload -w /System/Library/LaunchDaemons/org.apache.httpd.plist
```
