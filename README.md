# nginx

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
