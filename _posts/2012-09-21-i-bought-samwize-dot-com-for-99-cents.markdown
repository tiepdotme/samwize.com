---
layout: post
title: "I bought samwize.com for 99 cents"
date: 2012-09-21 10:51
comments: true
categories: [Blogging, Octopress]
---

This blog is now given an official (domain) name since I [switched to Octopress](/2012/09/10/switched-from-wordpress-to-octopress/) 2 weeks ago. 

The domain is bought from [NameCheap](http://www.namecheap.com/) at 99 cents for first year, thereafter $10.69/yr. This is a special promo given after they won the best registrar from lifehackers. (promo code **WELOVEU**)

Wonder how I host this blog?

<!-- more -->

## Host blog on Github Pages ##

The blogging framework is using Octopress, and the baked webpages are hosted using [Github Pages](/2012/09/11/how-to-setup-octopress-on-github-pages/) (Free!).

To use my custom domain, I added `samwize.com` to the file `/source/CNAME` in Octopress. 

Since I am now using a top-level domain, I have to add an A record pointing to `204.232.175.78`. I also redirect [www.samwize.com](http://www.samwize.com) to [samwize.com](http://samwize.com). My Namecheap host records looks like this:

| HOST NAME   | IP ADDRESS/URL    | RECORD TYPE       |
| ----------- | ----------------- | ----------------- |
| @           | 204.232.175.78    | A (Address)       
| www         | samwize.com.      | CNAME (Alias)     
