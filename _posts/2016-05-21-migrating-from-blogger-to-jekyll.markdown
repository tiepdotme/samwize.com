---
layout: post
title: "Migrating From Blogger to Jekyll"
date: 2016-05-24T21:51:00+08:00
categories: [Jekyll]
---

I had a Blogger site that was recently [migrated](http://just2me.com/2016/05/12/i-had-enough-with-blogger-com/) to the fabulous Jekyll (which this website is running on too!).

Jekyll provides migration tools for Blogger, Wordpress and others.

Export from Blogger to get all your posts in a XML file. eg. `blog-05-12-2016.xml`

Then run in your Jekyll folder:

    ruby -rubygems -e 'require "jekyll-import";
    JekyllImport::Importers::Blogger.run({ "source" => "./blog-05-12-2016.xml" })'

It will parse the XML and create the posts in your Jekyll folder.
