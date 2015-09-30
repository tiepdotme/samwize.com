---
layout: post
title: "How to setup multiple Wordpress (or domain) with 1 server instance"
date: 2014-05-17 14:53:06 +0800
comments: true
categories: [VPS, Wordpress]
---

After [migrating from a shared webhost to a dedicated VPS](/2014/05/16/migrating-from-shared-webhost-to-vps-for-wordpress/), the next step is to migrate all my wordpress sites to that 1 VPS instance.

This is basically setting up the wordpress scripts for each site, and writing the apache virtual host for each.

<!-- more -->

Digital Ocean have guides on [configuring multiple virtual hosts](https://www.digitalocean.com/community/articles/how-to-set-up-apache-virtual-hosts-on-ubuntu-12-04-lts) on 1 machine, and also how to [setup multiple wordpress manually](https://www.digitalocean.com/community/articles/how-to-set-up-multiple-wordpress-sites-on-a-single-ubuntu-vps).

I will be summarizing the steps.

# 1. Setup MySQL

Firstly, you need to setup your mysql database to be used with your wordpress.

In this example, we are using `wp_myblog1` for database name, `wp_myblog1` for the mysql user and `secretpassword`.

    mysql -u root -p

    # Setup your database name, and a mysql user
    CREATE DATABASE wp_myblog1;
    CREATE USER wp_myblog1@localhost;
    SET PASSWORD FOR wp_myblog1@localhost= PASSWORD("secretpassword");

    GRANT ALL PRIVILEGES ON wp_myblog1.* TO wp_myblog1@localhost IDENTIFIED BY 'secretpassword';
    FLUSH PRIVILEGES;
    exit


# 2. Setup wordpress scripts

Download and copy wordpress (the scripts) to some directory eg. `/var/www/myblog1.com`

    # Download Wordpress
    cd ~
    wget http://wordpress.org/latest.tar.gz
    tar xzvf latest.tar.gz

    cd /var/www
    sudo mkdir myblog1.com
    cp ~/wordpress/wp-config-sample.php ~/wordpress/wp-config.php
    sudo rsync -avP ~/wordpress/ /var/www/myblog1.com/

    cd /var/www/myblog1
    sudo chown www-data:www-data * -R
    sudo usermod -a -G www-data root


# 3. Configure Wordpress

Configure wp-config.php to let wordpress know how to connect to your mysql.

    cd /var/www/myblog1
    sudo nano wp-config.php

Edit these:

```php
define('DB_NAME', 'wp_myblog1');

define('DB_USER', 'wp_myblog1');

define('DB_PASSWORD', 'secretpassword');
```

Save and exit.



# 4. Setup Virtual Host

Setup Apache to let it know there are multiple virtual hosts to the machine.

    cd /etc/apache2/sites-available
    sudo cp 000-default.conf wp_myblog1.com.conf
    sudo nano wp_myblog1.com.conf

NOTE: You [must](http://www.epicfoobar.com/2013/08/apache-a2ensite-error-site-does-not-exist/) have a `.conf` extension

Edit as follows:

```xml
<VirtualHost *:80>
  ServerAdmin webmaster@myblog1.com
  ServerName myblog1.com
  ServerAlias *.myblog1.com

  DocumentRoot /var/www/myblog1.com

  <Directory />
    Options FollowSymLinks
    AllowOverride None
  </Directory>
  <Directory /var/www/myblog1.com>
    Options Indexes FollowSymLinks MultiViews
    AllowOverride None
    Order allow,deny
    allow from all
  </Directory>
</VirtualHost>
```

Enabled this new virtual host, and restart Apache

    sudo a2ensite myblog1.com.conf
    sudo service apache2 reload

That's it!

You have configured for 1 wordpress.

To configure for multiple wordpress, repeat with eg myblog2
















