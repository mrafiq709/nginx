# nginx load-balancer

```
sudo nano /etc/nginx/sites-available/load-balancer.conf

upstream backend {
	server 127.0.0.1:81;
	server 127.0.0.1:82;
	server 127.0.0.1:83;
}

# This server accepts all traffic to port 80 and passes it to the upstream.
# Notice that the upstream name and the proxy_pass need to match.

server {
	listen 80;

	server_name default.test;
	location / {
		proxy_pass http://backend;
	}
}

server {
    listen 81;

	root /home/rafiq/load-balancing/server1;

	# Add index.php to the list if you are using PHP
	index index.php index.html index.htm index.nginx-debian.html;

	server_name server1.test;
	location / {
		try_files $uri $uri/ /index.php$is_args$args;
	}
	# pass PHP scripts to FastCGI server
	#
	location ~ \.php$ {
			include fastcgi_params;
			fastcgi_intercept_errors on;
			fastcgi_index  index.php;
			# With php-fpm (or other unix sockets):
			fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
			fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
	}
}

server {
    listen 82;

	root /home/rafiq/load-balancing/server2;

	# Add index.php to the list if you are using PHP
	index index.php index.html index.htm index.nginx-debian.html;

	server_name server2.test;
	location / {
		try_files $uri $uri/ /index.php$is_args$args;
	}
	# pass PHP scripts to FastCGI server
	#
	location ~ \.php$ {
			include fastcgi_params;
			fastcgi_intercept_errors on;
			fastcgi_index  index.php;
			# With php-fpm (or other unix sockets):
			fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
			fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
	}

}

server {
    listen 83;

	root /home/rafiq/load-balancing/server3;

	# Add index.php to the list if you are using PHP
	index index.php index.html index.htm index.nginx-debian.html;

	server_name server3.test;
	location / {
		try_files $uri $uri/ /index.php$is_args$args;
	}
	# pass PHP scripts to FastCGI server
	#
	location ~ \.php$ {
			include fastcgi_params;
			fastcgi_intercept_errors on;
			fastcgi_index  index.php;
			# With php-fpm (or other unix sockets):
			fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
			fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
	}
}

```

create symlink with sites-enable

```
sudo ln -s /etc/nginx/sites-available/load-balancer.conf /etc/nginx/sites-enabled/load-balancer.conf
```

Reload nginx

```
systemctl reload nginx
```

##### Refference

https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/
