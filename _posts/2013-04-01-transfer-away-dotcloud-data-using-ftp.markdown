---
layout: post
title: "Transfer Away Dotcloud Data using FTP"
date: 2013-04-01 23:41
comments: true
categories: Dotcloud
---

If you have data on Dotcloud server that you want to transfer away via FTP directly, you can't, because Dotcloud don't provide FTP to the instances.

This is about the best way I know of transfering data, first by transferring to your own FTP server: 

<!-- more -->

```
// SSH to your dotcloud instance
dotcloud run www

// ZIP the data eg. html
tar -C $HOME -czf html.tar.gz ./current/html

// curl
curl -u myusername:mypassword -sST html.tar.gz ftp://myserver.com
```


