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

## security and ssl config

![alt text](image-61.png)
