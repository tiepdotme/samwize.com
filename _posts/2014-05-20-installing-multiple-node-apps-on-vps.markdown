---
layout: post
title: "Installing multiple node apps on VPS"
date: 2014-05-20 14:24:47 +0800
comments: true
categories: [node, VPS]
---

Digital Ocean has a guide on [installing node](https://www.digitalocean.com/community/articles/how-to-install-node-js-with-nvm-node-version-manager-on-a-vps), and another guide on installing [multiple node apps](https://www.digitalocean.com/community/articles/how-to-host-multiple-node-js-applications-on-a-single-vps-with-nginx-forever-and-crontab) on 1 instance.

This is my version, after setting up [apache and nginx together](/2014/05/19/running-nginx-with-apache-with-reverse-proxy/) on the same instance.

<!-- more -->

## Install node

Firstly, install `nvm` (node version manager), and also `git`.

The example use nvm version 0.7.0. You can [update accordingly](https://github.com/creationix/nvm).

```bash
cd ~
curl https://raw.githubusercontent.com/creationix/nvm/v0.7.0/install.sh | sh
source ~/.profile
apt-get install git
```

Then install `node`.

```bash
# List the versions
nvm ls-remote
# Assuming we want 0.10.28
nvm install 0.10.28
```

Finally, you probably want to copy the active version into `/usr/local`.

```
n=$(which node);n=${n%/bin/node}; chmod -R 755 $n/bin/*; sudo cp -r $n/{bin,lib,share} /usr/local
```


## Deploying your app

I have choose to deploy using SFTP.

I use Cyberduck (Mac app), and simply copy the node app into eg `/var/www/nodeapp.com/`

Note: There are better ways. And using [dokku](https://github.com/progrium/dokku) seems to be very popular, but dokku does not support running multiple apps.


## Setup Nginx

If you followed the [tutorial in setting up apache alongside nginx](/2014/05/19/running-nginx-with-apache-with-reverse-proxy/), you would want to edit the server block for the node app like this (I have changed the name to `nodeapp` instead of `nginxapp`):

```nginx /etc/nginx/sites-available/nodeapp.com.conf
    # Node server block
    server {
            listen  80;

            root  /var/www/nodeapp.com/;
            index   index.html index.htm;

            server_name   nodeapp.com;

            # Proxy to node app
            location / {
              proxy_pass http://localhost:8021;
              proxy_http_version 1.1;
              proxy_set_header Upgrade $http_upgrade;
              proxy_set_header Connection 'upgrade';
              proxy_set_header Host $host;
              proxy_cache_bypass $http_upgrade;
            }
    }
```

Restart nginx with `sudo service nginx restart`.


## Install Forever (Globally)

[Forever](https://github.com/nodejitsu/forever) is to ensure your node app gets run _forever_. That means if it crashes, it will restart for you.

```bash
npm install forever -g
```


## Start with Forever

```bash
cd /var/www/nodeapp.com/

# Install the packages, if you haven't
npm install

# Start forever, with minimum uptime 10 sec between launches if restart
# Assuming app.js is your application main script
forever start --spinSleepTime 10000 app.js
```


## Start with Forever, truly forever

Your app is not truly running forever.

If your VPS crash, then forever is not being started..

Hence, you need a cronjob and this `starter.sh` in your home folder. You can `nano ~/starter.sh` and copy the following:

```bash starter.sh
#!/bin/sh

if [ $(ps -e -o uid,cmd | grep $UID | grep node | grep -v grep | wc -l | tr -s "\n") -eq 0 ]
then
        export PATH=/usr/local/bin:$PATH
        forever start --spinSleepTime 10000 --sourceDir /var/www/nodeapp.com app.js >> /var/www/nodeapp.com/log.txt 2>&1
fi
```

Then `crontab -e`, and add the following:

```bash
@reboot /path/to/starter.sh
```

To know where your path to `starter.sh` is, use `pwd` at the directory.

That's it!

To test your script is correct, you can:

```
forever stopall
# Make sure the script can be executed
chmod +x starter.sh
~/starter.sh
forever list
# Should see 1 running process
```

Or reboot your VPS to see that it gets truly run forever and ever :)


## Multiple Node Apps

To deploy multiple node apps, repeat (except for installing node, git, forever, crontab).

Assuming I have another app call `nodeapp2`, these are the changes:

- `/var/www/nodeapp2.com/` - contains the node app
- `nodeapp2.com.conf` - for nginx configuration (with a different port)
- `starter.sh` - add another line for the new node app to forever start

