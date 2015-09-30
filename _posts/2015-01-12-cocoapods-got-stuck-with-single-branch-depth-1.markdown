---
layout: post
title: "Cocoapods got stuck with --single-branch --depth 1"
date: 2015-01-12 10:10:17 +0800
comments: true
categories: 
---

When I did a `pod update --verbose`, cocoapod got stuck when trying to clone my private repos.

With the verbose mode, this is the line it got stuck on:

    $ /usr/bin/git clone https://my.repos.com/my_library/ /MyWorkspace/ --single-branch --depth 1

Turned out it is an [issue to do with using https](https://github.com/CocoaPods/CocoaPods/issues/2481) to connect with the repository.

Solution is to change to SSH counterpart.
