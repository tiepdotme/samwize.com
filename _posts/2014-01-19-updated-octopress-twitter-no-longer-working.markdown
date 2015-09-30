---
layout: post
title: "Updated Octopress - twitter no longer working"
date: 2014-01-19 16:55:31 +0800
comments: true
categories: Octopress
---

After [15 months](/2012/09/10/switched-from-wordpress-to-octopress/) of using Octopress, I updated the Greyshade theme I initially used, and also fixed some broken stuff with Octopress.

<!-- more -->

### Twitter no longer working

Twitter API moved forward, from version 1.0 a year ago to now version 1.1.

The big change is the use of OAuth even for fetching public timeline. They [retired the easy to use basic auth](https://blog.twitter.com/2013/api-v1-retirement-update).. This broke the Twitter status that is displayed at the top of the Greyshade theme I was using.

Hence, I changed the layout and removed twitter from the header. Not a big deal to lose this 'feature'.


### Date is not displayed in post

I have no idea why Greyshade doesn't display the date and category of a post when you are viewing the post in full. These meta are shown only in homepage view.

Anyway, I added in the date and category.


### Forked Greyshade

Unfortunately, I think [Shashank](http://shashankmehta.in/archive/2012/greyshade.html) is no longer actively maintaining his Octopress Greyshade theme, since he migrated to pure Jekyll.

Hence I [forked my own version](https://github.com/samwize/greyshade/tree/master2) with the above 2 changes. The branch is `master2`, which didn't merge in the style changes from various authors, as I don't agree with their style change.
