---
layout: post
title: "Running Nginx with Apache, with reverse proxy"
date: 2014-05-19 12:20:28 +0800
comments: true
categories: [VPS]
---

When I [migrate](/2014/05/16/migrating-from-shared-webhost-to-vps-for-wordpress/) my wordpress-es to VPS, Apache was the default setup.

Problem: I wanted to run some Node.js apps in the same VPS, but node requires nginx.

Hence, I configured my VPS to run **both Apache and Nginx**.

<!-- more -->

That's possible, when you make nginx a reverse proxy server.

The plan is as such:

- nginx listens to **port 80**
- nginx 1st server block is configured for node app at **port 8021**
- nginx 2nd server block is configured to proxy to apache at **port 8011**
- apache listen to port 8011
- apache's virtual hosts listen to port 8011

The details and a sample of the `.conf` files as follows. My scenario is using `nginxapp.com` and `apacheapp.com`


## Nginx

There are 2 server blocks, which I put in 2 seperate conf files: `nginxapp.com.conf` and `apache-proxy.conf`.

```nginx /etc/nginx/sites-available/nginxapp.com.conf
    # Node server block
    server {
            listen  80;

            root  /var/www/nginxapp.com/;
            index   index.html index.htm;

            server_name   nginxapp.com;

            # temp for serving static
            location / {
              try_files $uri $uri/ /index.php;
            }

            # Proxy to node app
            # Uncomment and remove the temp above if your node app is setup at port 8021
            #location / {
            #  proxy_pass http://localhost:8021;
            #  proxy_http_version 1.1;
            #  proxy_set_header Upgrade $http_upgrade;
            #  proxy_set_header Connection 'upgrade';
            #  proxy_set_header Host $host;
            #  proxy_cache_bypass $http_upgrade;
            #}
    }
```

Note: You should setup your node app and listen on 8021. In the meanwhile (testing), you can put a dummy index.html in `/var/www/nginxapp.com/`.

```nginx /etc/nginx/sites-available/apache-proxy.conf
    # Apache server block
    # Proxy to 8011 which Apache is listening to
    server {
            listen  80 default_server;

            location / {

                    proxy_set_header X-Real-IP  $remote_addr;
                    proxy_set_header X-Forwarded-For $remote_addr;
                    proxy_set_header Host $host;
                    proxy_pass http://localhost:8011;

             }

             location ~ /\.ht {
                    deny all;
            }
    }
```

Enable the nginx conf with these commands:

```bash
sudo ln -s /etc/nginx/sites-available/nginxapp.com.conf /etc/nginx/sites-enabled/nginxapp.com.conf
sudo ln -s /etc/nginx/sites-available/apache-proxy.conf /etc/nginx/sites-enabled/apache-proxy.conf
```


## Apache

For apache, you need to change the port file to listen to 8011.

```apache /etc/apache2/ports.conf
    Listen 8011

    # As per default configuration
```

Below is a sample of a virtual host file. You need to change for all. The change is again on the listening port.

```apache /etc/apache2/sites-available/awesome.com.conf
    <VirtualHost *:8011>
        # as per your configuration
    </VirtualHost>
```


## Restart

With that, restart each service with apache first, then nginx.

```bash
sudo service apache restart
sudo service nginx restart
```

Now, you if you go to `http://apacheapp.com`, it should work, and it is because nginx is proxying nicely to apache.

You can also go to `http://apacheapp.com:8011`, which is directly to apache.

And of course, you can now write your node app at `http://nodeapp.com`.



## Other guides

I referenced from some of Digital Ocean guides:

- [Installing nginx](https://www.digitalocean.com/community/articles/how-to-install-nginx-on-ubuntu-12-04-lts-precise-pangolin)
- [Nginx as a frontend proxy](https://www.digitalocean.com/community/articles/how-to-configure-nginx-as-a-front-end-proxy-for-apache)
- [Hosting multiple node apps](https://www.digitalocean.com/community/articles/how-to-host-multiple-node-js-applications-on-a-single-vps-with-nginx-forever-and-crontab)

