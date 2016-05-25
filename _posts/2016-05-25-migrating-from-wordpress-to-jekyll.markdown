---
layout: post
title: "Migrating From Wordpress to Jekyll"
date: 2016-05-25T21:50:52+08:00
categories: [Jekyll]
---

Recently I migrated [blog.just2us.com](http://blog.just2us.com) from a self hosted wordpress to Jekyll on Github Pages.

The steps I took for migration is very different from the previous guide on [migrating from Blogger](/2016/05/24/migrating-from-blogger-to-jekyll/) to Jekyll.


## What's wrong with Jekyll Import

Jekyll provided it's own [migration tool for Wordpress](https://import.jekyllrb.com/docs/wordpress/).

However, the tool has 2 major problems:

- it does not export the images
- it export the html wholesale, without converting to markdown

By not exporting the images, the tool depends on linking all images url back to to original site. That means after migration, you will need to maintain the original site in order for the images to work!

I wouldn't want that, because my migration intend to keep the same base URL.


## WP Plugin: Jekyll Exporter

Fortunately, there is a better way in the form of a [Wordpress plugin](https://github.com/benbalter/wordpress-to-jekyll-exporter/).

Simply install the plugin, then go to Tools > Export to Jekyll. However, this likely wouldn't work because of timeout etc.

The better way is ssh in and cd to the plugin directory eg. `/var/www/just2us.com/wp-content/plugins/jekyll-exporter` then run:

    php jekyll-export-cli.php > jekyll-export.zip    
    
If there is no error, then the zip will contain all the markdowns and images!
    
    
## Encountered out of memory in PHP

For my case, I encountered the error:

    PHP Fatal error:  Allowed memory size of 134217728 bytes exhausted (tried to allocate 89391104 bytes)
    
There is a permanent and a temporary way to fix this, and I choose a [temporary solution](http://stackoverflow.com/a/22283701/242682).

I edited `jekyll-export-cli.php` and added:

    ini_set('memory_limit', '-1');

This will overide the default limit.

Now run `php jekyll-export-cli.php > jekyll-export.zip` again, and it should be fine.
