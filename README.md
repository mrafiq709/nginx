# nginx

Step 1 – Installing Nginx
--------------------------
```
sudo apt update
sudo apt install nginx
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
See error Log
--------------
```
sudo tail -30 /var/log/nginx/error.log
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
