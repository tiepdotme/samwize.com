---
layout: post
title: "Migrating from shared webhost to VPS (for Wordpress)"
date: 2014-05-16 23:33:49 +0800
comments: true
categories: [VPS, Wordpress]
---

After many years of using a shared webhost, I finally decided to switch to VPS.

The shared webhost I have been using is HostGator, which has been good. But a VPS is even better, and so I switched to [Digital Ocean](https://www.digitalocean.com/?refcode=69baaaf5a07b), which cost only $5 a month (I was paying $10 to HostGator for just a shared webhost)!

The Wordpress I was migrating is [blog.just2us.com](http://blog.just2us.com) (not this blog). As you might know, this blog is hosted on [github using Octopress](http://samwize.com/2012/09/11/how-to-setup-octopress-on-github-pages/).

<!-- more -->

# Why switch to Digital Ocean ?

I was inspired by [Marco's blog on web hosting](http://www.marco.org/2014/03/27/web-hosting-for-app-developers).

He urged web developer to use VPS, instead of Amazon EC2, or other cloud providers. Shared webhost is lower than these, so I decided to jump to a new level.

At the same time, I was playing around with some small projects, and I was disgusted because:

- Heroku doesn't have Singapore instance
- Amazon support Singapore as a region, but I was chalking up $15 per month for very low usage
- And my HostGator shared webhost is under utilized

And so I decided to switch and become a better developer.


# Setting up Digital Ocean

[Setting up](https://www.digitalocean.com/community/articles/one-click-install-wordpress-on-ubuntu-13-10-with-digitalocean) is very easy, as they promised. I setup Wordpress on Ubuntu 13.10, with the smallest instance in Singapore.

Yet even their smallest instance is 512MB Ram, with 20GB **SSD Disk**! That's for $5 a month only.

And it is running very fast.


# Migrate Wordpress the wrong way

Wordpress has it's own Import/Export function under Tools.

However, that only import/export items like posts etc, and does NOT include themes and others.

That is the WRONG way to migrate as it doesn't include lots of other stuff.

But if you choose to import/export via the WXR XML file, you might run into the 2MB limit. The solution listed on [codex](http://codex.wordpress.org/FAQ_Working_with_WordPress#How_do_I_Import_a_WordPress_WXR_file_when_it_says_it_is_too_large_to_import.3F) is too much. Too much to read. Too extreme solution.

You should simply split up the files to smaller chunk, either manually (if you understand XML syntax), or using a [file splitter](http://suhastech.com/wordpress-wxr-xmlfile-splitter-for-mac-os-x/).


# Migrate Wordpress the right way

[Smashing Magazine](http://www.smashingmagazine.com/2013/04/08/moving-wordpress-website/) has a good guide on migrating.

In short:

    1. Old site > Settings > turn off permalinks

    2. Old site > phpMyAdmin > Export SQL

    3. New site > phpMyAdmin > Import SQL (manually split the file, if too big)

    4. SFTP transfer "wp-content" from old site to new site

    5. Change your DNS records to point to your droplet IP

Pitfall: Make sure you copy everything (your themes and plugins) in `wp-content`, or you could face the white screen of death when you visit your new site.


# Other Pitfalls

### Search & Replace DB

This [script](https://github.com/interconnectit/Search-Replace-DB) is awesome for replacing eg URL. Migration doesn't cover changing the content of your database, hence this script helps tremendously in replacing thousands of rows easily.

Download, unzip, upload to your wordpress root folder, then go to http://your.wordpress.com/Search-Replace-DB/, and enter what you want to replace.

Once done, remember to delete the folder. Otherwise anyone could go in and cause havoc!


### Upload Error

If you upload image and faced an error `Unable to create directory wp-content/uploads/2014/05. Is its parent directory writable by the server?`, then it is most likely due to [upload_path is wrong](http://stackoverflow.com/questions/20948586/wordpress-3-8-image-upload-is-not-possible).

Go to phpMyAdmin > wp_options table > upload_path field > change to "wp-content/uploads".



