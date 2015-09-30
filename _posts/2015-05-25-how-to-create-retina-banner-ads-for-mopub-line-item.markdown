---
layout: post
title: "How to create retina banner ads for MoPub line item"
date: 2015-05-25 16:04:49 +0800
comments: true
categories: [iOS]
---

If you have been using MoPub to serve your banner ads eg. for backfills, it is important that you know it is possible to serve retina sizes @2x.

This means you can upload 640x100 instead of 320x50.

MoPub documentation has been poor to highlight this capability. It is found only in their [forum](https://twittercommunity.com/t/is-it-possible-to-have-banner-ads-optimized-for-retina-devices/6914).

<!-- more -->

Anyhow, the steps as follows:

1. Create a new creative with the 320x50 banner format
2. Set the creative type to **Image**, and upload your retina image file
3. Save and pause this creative
4. Preview on the paused creative, right click and copy the image URL 
5. Create another NEW creative, but set the creative type to **HTML**
6. Copy the following into the HTML section:

    ```
    <a href="CLICK URL HERE" target="_blank"> <img src="IMAGE URL HERE" width=100%> </a>
    ```

It is a straight forward `<a>` tag, which makes the image fills the width of the banner.

Note: The paused creative is simply to get the image URL, which is used by the actual creative using HTML mode. Do not delete the paused creative.

