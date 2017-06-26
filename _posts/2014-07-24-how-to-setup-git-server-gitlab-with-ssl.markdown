---
layout: post
title: "How to setup git server (GitLab), with SSL"
date: 2014-07-24 16:34:37 +0800
comments: true
categories: [VPS, Git]
---

I have been using [unfuddle](https://unfuddle.com) which provided me with free 512 MB to use for my private projects, and (of course) [GitHub](https://github.com) for any opensource projects.

But what is better than setting up Git on your own server?

I have a [$5/mth Digital Ocean VPS](https://www.digitalocean.com/?refcode=69baaaf5a07b) that is overloaded to run [node apps](http://samwize.com/2014/05/20/installing-multiple-node-apps-on-vps/), [wordpress](http://samwize.com/2014/05/19/running-nginx-with-apache-with-reverse-proxy/) and [mail server](http://samwize.com/2014/07/11/setting-up-an-email-server-for-multiple-subdomains-on-digital-ocean/).. So, I might as well run [GitLab](http://gitlab.com) too.

<!-- more -->

## GitLab Installation Guide

At the point of writing, I am using **GitLab 7.1 (stable)** on **Ubuntu 14.04**.

Refer to their detailed [installation guide](https://github.com/gitlabhq/gitlabhq/blob/7-1-stable/doc/install/installation.md).

WARNING: Do NOT use their [simplified guide](https://about.gitlab.com/downloads/) which install everything automatically. I did that, and it screwed up my existing nginx config..

Their [installation guide](https://github.com/gitlabhq/gitlabhq/blob/7-1-stable/doc/install/installation.md) is long, but comprehensive, and you can choose not to install any packages which you already have installed.

Follow their guide (except on the sections on HTTPS/SSL).

## Setting up for HTTPS/SSL

For setting up HTTPS/SSL, refer to this [GitLab recipe](https://gitlab.com/gitlab-org/gitlab-recipes/tree/master/misc/ssl-certificate-implemented).

But I still need some refinement to that recipe, so I will share my configs.

I will be setting up for a fictional domain `git.okloh.com`.

### Create your FREE SSL Cert

A FREE class 1 SSL is provided by StartCom. Go to [StartCom](https://www.startssl.com/), and create another certificate for `git.okloh.com`.

1. Save your private key password somewhere

2. On your server, `cd /etc/ssl/private`

3. Save your private key `git-okloh.key`

4. Save your certificate `git-okloh.pem`

5. `wget https://www.startssl.com/certs/sub.class1.server.ca.pem` to download their intermediate CA Cert

6. `cat git-okloh.pem sub.class1.server.ca.pem > git-okloh-chain.pem` to concat and create a chain cert

7. `openssl rsa -in git-okloh.key -out git-okloh-decrypted.key` > enter your private key password

Note: In many tutorials, you would see that they use a `.crt` instead of a `.pem`. They are the same thing, but in [different format](http://info.ssl.com/article.aspx?id=12149). But it doesn't matter. You can use a `.pem` in place.

### Nginx

This is my nginx config that works:

```nginx 
# /etc/nginx/sites-enabled/gitlab
upstream gitlab {
  server unix:/home/git/gitlab/tmp/sockets/gitlab.socket;
}

server {
  listen 80;
  server_name   git.okloh.com;
  rewrite       ^ https://$server_name$request_uri? permanent;
}

server {
  listen        443 ssl;
  server_name   git.okloh.com;
  root          /home/git/gitlab/public;

  ssl                   on;
  ssl_certificate       /etc/ssl/private/git-okloh-chain.pem;
  ssl_certificate_key   /etc/ssl/private/git-okloh-decrypted.key;
  ssl_prefer_server_ciphers on;
  ssl_protocols         SSLv3 TLSv1;
  ssl_ciphers           ALL:!aNULL:!ADH:!eNULL:!LOW:!EXP:RC4+RSA:+HIGH:+MEDIUM;

  client_max_body_size 20m;

  access_log  /var/log/nginx/gitlab_access.log;
  error_log   /var/log/nginx/gitlab_error.log;

  location / {
    try_files $uri $uri/index.html $uri.html @gitlab;
  }

  location @gitlab {
    proxy_read_timeout 300; # https://github.com/gitlabhq/gitlabhq/issues/694
    proxy_connect_timeout 300; # https://github.com/gitlabhq/gitlabhq/issues/694
    proxy_redirect     off;

    proxy_set_header   X-Forwarded-Proto $scheme;
    proxy_set_header   Host              $http_host;
    proxy_set_header   X-Real-IP         $remote_addr;
    proxy_set_header   X-Forwarded-For   $proxy_add_x_forwarded_for;

    proxy_pass http://gitlab;
  }

  location ~ ^/(assets)/  {
    root /home/git/gitlab/public;
    gzip_static on; # to serve pre-gzipped version
    expires max;
    add_header Cache-Control public;
  }

  error_page 502 /502.html;
}
```

### GitLab Config

```yml 
# /home/git/gitlab/config/gitlab.yml
gitlab:
  host: git.okloh.com
  port: 443
  https: true
```

### GitLab-Shell Config

```yml 
# /home/git/gitlab-shell/config.yml
gitlab_url: "https://git.okloh.com/"
http_settings:
  self_signed_cert: false
ca_file: "/etc/ssl/private/git-okloh-chain.pem"
ca_path: "/etc/ssl/private"
```

### Restart

Remember to restart both services.

```bash
sudo service gitlab restart
sudo service nginx restart
```

If anything is wrong, you can check:

```bash
cd /home/git/gitlab/
sudo -u git -H bundle exec rake gitlab:env:info RAILS_ENV=production
sudo -u git -H bundle exec rake gitlab:check RAILS_ENV=production
```

### Pitfall: Port 443 already binded

In a [previous tutorial](http://samwize.com/2014/07/11/setting-up-an-email-server-for-multiple-subdomains-on-digital-ocean/), I setup iRedMail, which actually runs on Apache, which listen on port 443.

You will need to remove port 443 on apache, and probably do a reverse proxy from nginx for port 443. _This perhaps will be covered in another tutorial._

### Pitfall: Sidekiq not running

```
# Make sure it is running
ps -ef | grep sidekiq | grep -v grep

# If not, run it
sudo -u git -H RAILS_ENV=production bin/background_jobs start
```
