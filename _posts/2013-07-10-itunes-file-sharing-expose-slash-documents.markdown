---
layout: post
title: "iTunes File Sharing - Expose /Documents"
date: 2013-07-10 00:32
comments: true
categories: iOS
---

iOS introduced a way for users to transfer files from an app to a computer via iTunes.

It is simply adding a `UIFileSharingEnabled` to info.plist. There is a very small introduction [here](http://developer.apple.com/library/ios/#technotes/tn2152/_index.html).

<!-- more -->

When you enable file sharing, ALL files in `<Application_Home>/Documents` will be shared. 

Users can copy files to and from the app, as well as **delete**.

So be cautious of that.

You might also want to monitor the directory. There is [a way](http://www.mlsite.net/blog/?p=2312) to do that using `kqueue()`.