---
layout: post
title: "Using Cocoapods in a multiple targets project"
date: 2015-01-12 18:22:12 +0800
comments: true
categories: 
---

If you have multiple targets in an Xcode project, then you will need to use `link_with` in your `Podfile` to specify the targets that will use the pod.

The [documentation of link_with](http://guides.cocoapods.org/syntax/podfile.html#link_with) explains the usage. For example to link with your 2 targets:

    link_with 'Target1', 'Target2'

Without specifying all the targets, the default behaviour of Cocoapods is to use the first target as default, and the rest of the targets will not include the pod libraries. 
