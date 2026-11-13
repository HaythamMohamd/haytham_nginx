## Nginx

# install nginx

```bash
apt-get update
apt-get install nginx
systemctl start nginx
systemctl status nginx

cd /etc/nginx
 ⚡  root  ls
conf.d        fastcgi_params  koi-win     modules-available  nginx.conf    scgi_params      sites-enabled  uwsgi_params
fastcgi.conf  koi-utf         mime.types  modules-enabled    proxy_params  sites-available  snippets       win-utf
```

# nginx configuration file

![alt text](image.png)

![alt text](image-1.png)

## site available

```bash
# here all the sites on nginx 
/etc/nginx/sites-available

# here the static files
/var/www/html
```

## create a new site

```bash
# 01 go to sites-availbel
cd /etc/nginx/sites-available

# 02 copy the default to the name of your site and remove the default
cp default haytham.conf
rm default

# edit haytham.conf 
vim  haytham.conf
======                                                                                                                                                                                                       server {                                                                                                                                                                                                           listen 80 ;                                                                                                                                                                                                listen [::]:80 ;                                                                                                                                                       root /var/www/haytham;
server_name www.haytham.com;

# 03 go to sites-enabled
cd /etc/nginx/sites-enabled

# 04 make soft link for /etc/nginx/sites-available/haytham.conf here
ln -s /etc/nginx/sites-available/haytham.conf

# 05 check that everything is ok
nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful

# 06 reload the configuration
nginx -s reload
# or
systemctl reload nginx

# 07 create a dir for /var/www/html/haytham
mkdir /var/www/haytham

# 08 create index.html
cat /var/www/haytham/index.html
<center>Welcome ya haytham</center>

# 09 test 
curl localhost
```

## reverse proxy

![alt text](image-2.png)

```bash
# 01 copy haytham.conf
cp haytham.conf yunus.conf

# 02 edit yunus.conf

        ## comment this
        #root /var/www/haytham;

        # Add index.php to the list if you are using PHP
        index index.html index.htm index.nginx-debian.html;

        server_name www.yunus.com;

        location / {
                proxy_pass http://54.85.90.72;
        }                                                                                                                                           

# 03 enable yunus.conf
ln -s ../sites-available/yunus.conf

# 04 check configuration and reload
nginx -t
systemctl reload nginx
```

## load balancer

```bash
# 01- copy the file
cp haytham.conf lb.conf

# 02 edit lb.conf
vim /etc/nginx/sites-available/lb.conf
======================================

upstream backend_app {
        server node01.local;
        server node02.local;
}
# Default server configuration
#
server {
        listen 80 ;
        listen [::]:80 ;
======================================
# 03 go to lb.conf
        ## comment this
        #root /var/www/haytham;

        # Add index.php to the list if you are using PHP
        index index.html index.htm index.nginx-debian.html;

        server_name www.haytham.com;

        location / {
                proxy_pass http://backend_app;
        }                                                                                                                                         
```

## weighted round robin

```bash

upstream backend_app {
        server node01.local weight=4;
        server node02.local weight=1;
}
# Default server configuration
#
server {
        listen 80 ;
        listen [::]:80 ;
```

## ip hash

```bash

upstream backend_app {
        ip_hash;
        server node01.local ;
        server node02.local ;
}
# Default server configuration
#
server {
        listen 80 ;
        listen [::]:80 ;
```

### nginx crash course

![alt text](image-3.png)
![alt text](image-4.png)
![alt text](image-5.png)
![alt text](image-6.png)

![alt text](image-7.png)

![alt text](image-8.png)
![alt text](image-9.png)

![alt text](image-10.png)

## install nginx on centos

![alt text](image-11.png)
![alt text](image-12.png)
![alt text](image-13.png)
![alt text](image-14.png)

# open firewall

![alt text](image-15.png)

# set selinux

![alt text](image-16.png)

# test it

![alt text](image-17.png)

## nginx configuration managment

![alt text](image-18.png)

# config dir and file

![alt text](image-19.png)

# nginx config is devided to these sections

![alt text](image-20.png)

# here the global

![alt text](image-21.png)

# to define how many connections for each worker

![alt text](image-22.png)

# access logs and error logs

![alt text](image-23.png)

# document root and permissions

![alt text](image-24.png)
![alt text](image-25.png)

# error page

![alt text](image-26.png)

# virtual hosts

![alt text](image-27.png)

# name based virtual hosting

![alt text](image-28.png)
![alt text](image-29.png)

# tips for multiple environment for prod => caching and for test => debug

![alt text](image-30.png)

# rewrite and return

![alt text](image-31.png)

## LAB

![alt text](image-32.png)

# LAB solution

# 01 got to nginx.conf

![alt text](image-33.png)

# 02 added warn at error logs

![alt text](image-34.png)

# 03 create /var/www/example.com

![alt text](image-35.png)
![alt text](image-36.png)

# 04 make permissions on it

![alt text](image-37.png)

# 05 create a config file for it

![alt text](image-38.png)
![alt text](image-39.png)

# 06 create 404 error page for example.com site

![alt text](image-40.png)
![alt text](image-41.png)

# 07 create test.com site

![alt text](image-43.png)
![alt text](image-42.png)

# 08 create test.com.conf

![alt text](image-44.png)
![alt text](image-45.png)

# 09 put the perssions

![alt text](image-46.png)

# 10 to make rewrite to any request go to test.com should go to example.com

![alt text](image-47.png)
![alt text](image-48.png)

# 11 create oldsite.com.conf so any request go to oldsite.com will be redirect to example.com

![alt text](image-49.png)
![alt text](image-50.png)

# 12 check firewall

![alt text](image-51.png)

# 13 create /etc/hosts

![alt text](image-52.png)

# 14 check nginx config and reload

![alt text](image-53.png)
![alt text](image-54.png)

# 15 check from browser

![alt text](image-55.png)
![alt text](image-56.png)
![alt text](image-57.png)

# 15.1 redirectction from exmaple.com/oldpage  ==> exmaple.com/newpage

![alt text](image-58.png)
![alt text](image-59.png)

# 15.2 redirectction from oldsite.com  ==> exmaple.com

![alt text](image-60.png)

# LAB SUMMARY
```yml

[root@ip-172-31-19-159 ~]# hostnamectl set-hostname nginx
[root@ip-172-31-19-159 ~]# exit
logout
[ec2-user@ip-172-31-19-159 ~]$ sudo -i
[root@nginx ~]# 
[root@nginx ~]# 
[root@nginx ~]# 
[root@nginx ~]# 
[root@nginx ~]# 
[root@nginx ~]# dbf install -y nginx

[root@nginx ~]# systemctl start nginx
[root@nginx ~]# ^start^enable
systemctl enable nginx
Created symlink /etc/systemd/system/multi-user.target.wants/nginx.service → /usr/lib/systemd/system/nginx.service.
[root@nginx ~]# 
[root@nginx ~]# 
[root@nginx ~]# curl localhost
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, nginx is successfully installed and working.
Further configuration is required for the web server, reverse proxy, 
API gateway, load balancer, content cache, or other features.</p>

<p>For online documentation and support please refer to
<a href="https://nginx.org/">nginx.org</a>.<br/>
To engage with the community please visit
<a href="https://community.nginx.org/">community.nginx.org</a>.<br/>
For enterprise grade support, professional services, additional 
security features and capabilities please refer to
<a href="https://f5.com/nginx">f5.com/nginx</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>


[root@nginx nginx]# cat /etc/nginx/nginx.conf
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log notice;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    keepalive_timeout   65;
    types_hash_max_size 4096;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;

    server {
        listen       80;
        listen       [::]:80;
        server_name  _;
        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        error_page 404 /404.html;
        location = /404.html {
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }



[root@nginx ~]# mkdir -p /var/www/example.com
[root@nginx ~]# echo "example.com site" > /var/www/example.com/index.html
[root@nginx ~]# chown -R nginx:nginx /var/www/example.com/index.html
[root@nginx ~]# vim /etc/nginx/conf.d/example.com.conf
[root@nginx ~]# vim /var/www/example.com/404.html
server {
        listen 80;
        server_name example.com;
        root /var/www/example.com;
        index index.html;

        # custom 404 error page
        error_page 404 /404.html;
        location = /404.html {
                root /var/www/example.com;
        }

        # Redirection /oldpage to /newpage 
        location /oldpage {
                rewrite ^/oldpage$ /newpage permanent;
        }
}

[root@nginx ~]# mkdir -p /var/www/test.com
[root@nginx ~]# echo "test.com site" > /var/www/test.com/index.html
[root@nginx ~]# cp /etc/nginx/conf.d/example.com.conf /etc/nginx/conf.d/test.com.conf
[root@nginx ~]# vim /etc/nginx/conf.d/test.com.conf
server {
        listen 80;
        server_name test.com;
        root /var/www/test.com;
        index index.html;

        # enable directory listing for /files
        location /files/ {
                autoindex on;
        }
}

[root@nginx ~]# chown -R nginx:nginx /var/www/test.com/


[root@nginx ~]# cp /etc/nginx/conf.d/example.com.conf /etc/nginx/conf.d/oldsite.com.conf
[root@nginx ~]# cim /etc/nginx/conf.d/oldsite.com.conf
[root@nginx ~]# vim /etc/nginx/conf.d/oldsite.com.conf
server {
        listen 80;
        server_name oldsite.com;
        return 301 http://example.com$request_uri;
}


[root@nginx ~]# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
[root@nginx ~]# 
[root@nginx ~]# 
[root@nginx ~]# systemctl restart nginx
[root@nginx ~]# 
[root@nginx ~]# curl localhost
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, nginx is successfully installed and working.
Further configuration is required for the web server, reverse proxy, 
API gateway, load balancer, content cache, or other features.</p>

<p>For online documentation and support please refer to
<a href="https://nginx.org/">nginx.org</a>.<br/>
To engage with the community please visit
<a href="https://community.nginx.org/">community.nginx.org</a>.<br/>
For enterprise grade support, professional services, additional 
security features and capabilities please refer to
<a href="https://f5.com/nginx">f5.com/nginx</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
[root@nginx ~]# 
[root@nginx ~]# 
[root@nginx ~]# curl example.com
example.com site
[root@nginx ~]# curl test.com
test.com site
[root@nginx ~]# curl example.com/non
<h1>404 Not found </h1>
<p> the page that you are looking for is not exist </p>
[root@nginx ~]# curl example.com/oldpage
<html>
<head><title>301 Moved Permanently</title></head>
<body>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>nginx/1.30.6</center>
</body>
</html>
[root@nginx ~]# curl example.com/newpage
<h1>404 Not found </h1>
<p> the page that you are looking for is not exist </p>
```
## security and ssl config

![alt text](image-61.png)
![alt text](image-62.png)
![alt text](image-63.png)
![alt text](image-64.png)

# encryption , authentication and integrity

![alt text](image-65.png)
![alt text](image-66.png)

# symmetric and asymmetric encryption

![alt text](image-67.png)

# https

![alt text](image-68.png)
![alt text](image-69.png)

# why https is important?

![alt text](image-70.png)
![alt text](image-71.png)
![alt text](image-72.png)
![alt text](image-73.png)
![alt text](image-74.png)

# how does ssl/tls work?

![alt text](image-75.png)
![alt text](image-76.png)
![alt text](image-77.png)
![alt text](image-78.png)
![alt text](image-79.png)
![alt text](image-80.png)

# what is ssl/tls cert ?

![alt text](image-81.png)

# types of certs

![alt text](image-82.png)

# how to generate self signed cert using openssl

![alt text](image-83.png)

# how to configure it at the nginx cert

![alt text](image-84.png)

# diffi-helman paramters

![alt text](image-85.png)

# to make a cert using lets encrypt (certbot)

![alt text](image-86.png)

# here to enforce https and do the redirection at nginx

![alt text](image-87.png)

# here if you want to use ssl from begining

![alt text](image-88.png)

# ssl optimization

![alt text](image-89.png)

## LAB for ssl

![alt text](image-90.png)

# LAb Solution

# 01 create self signed certs

![alt text](image-91.png)

# 02 copy the certs(exampl.com.crt , example.com.key and rootCA.crt) to /etc/ssl/certs

![alt text](image-92.png)

# 03 do the redirection, anyone request http will be redirect to https

![alt text](image-97.png)
![alt text](image-94.png)

# 04 creare another block for the https

![alt text](image-95.png)

# 05 to make optimization, delet the old headers can ciphers

![alt text](image-96.png)

# 06 check the firewall and config then reload

![alt text](image-98.png)

# 07 test using curl of browser

# http will be redirected to https

![alt text](image-101.png)
![alt text](image-99.png)
![alt text](image-100.png)

# https will open

![alt text](image-102.png)

## nginx as reverse proxy and load balancer

![alt text](image-103.png)

# reverse proxy

![alt text](image-104.png)

# benefits of using reverse proxy

![alt text](image-105.png)
![alt text](image-107.png)
![alt text](image-109.png)
![alt text](image-110.png)

# how to make nginx to be reverse proxy

![alt text](image-111.png)

# you can set custom headers

![alt text](image-112.png)
![alt text](image-113.png)

## nginx as load balancer

# 01 round robin

![alt text](image-114.png)
![alt text](image-115.png)

# 02 least connections

![alt text](image-116.png)
![alt text](image-117.png)
![alt text](image-118.png)

# 03 ip hash

![alt text](image-119.png)
![alt text](image-120.png)

# 04 weighted load balancing

![alt text](image-121.png)
![alt text](image-122.png)

# 05 least time (nginx plus)

![alt text](image-123.png)
![alt text](image-124.png)

## health checks

![alt text](image-125.png)

# types of health checks

![alt text](image-126.png)

## caching in nginx

![alt text](image-127.png)
![alt text](image-128.png)

## LAB

![alt text](image-130.png)

# LAB Solution

# 01 go to the config file

![alt text](image-131.png)

# 02 create the pool

![alt text](image-132.png)

# 03 added some headers

![alt text](image-133.png)

# 04 create the cache

![alt text](image-134.png)

# 05 create block for ssl

![alt text](image-135.png)

# 06 check the firewall and config

![alt text](image-136.png)
![alt text](image-137.png)
![alt text](image-138.png)
![alt text](image-139.png)

# 07 test using curl

# -k to enforce it using self sign cert

![alt text](image-141.png)

# this should be http not https

![alt text](image-140.png)
![alt text](image-142.png)

# test round robin

![alt text](image-143.png)

## advanced topcis and performance tunning

![alt text](image-144.png)

# 01 worker_processes auto if ou don't know number of cpu

![alt text](image-145.png)

# 02 optimize buffer size

![alt text](image-146.png)

# 03 managing timeout

![alt text](image-147.png)

# 04 sendfile

![alt text](image-148.png)

# 05 rate limiting

![alt text](image-149.png)
![alt text](image-150.png)

## logging and monitoring

# log rotate

![alt text](image-151.png)

# monitoring

![alt text](image-152.png)

## LAB Solution

# 01 check the processes

![alt text](image-157.png)
![alt text](image-153.png)

# 02 worker_connections, it is better to be more depending on resources

![alt text](image-154.png)

# 03 for tunning make the body size 10M

![alt text](image-155.png)

# 04 for client_header_timeout

![alt text](image-156.png)

# 05 configure rate limiting on the config of example.com

![alt text](image-158.png)
![alt text](image-159.png)

# 06 create custom log

![alt text](image-162.png)
![alt text](image-161.png)

# 07 create logrotate file

![alt text](image-164.png)
![alt text](image-163.png)
![alt text](image-165.png)
