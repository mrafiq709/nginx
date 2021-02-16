# nginx

Processing php file:
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
