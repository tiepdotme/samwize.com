---
layout: post
title: "Difference between Build and Version in iOS"
date: 2014-04-11 11:28:21 +0800
comments: true
categories: iOS
---

This is very well [answered in StackOverflow](http://stackoverflow.com/a/6965086/242682).

```objc
// Version is the readable dot notation eg `1.2.3` or `5.0`
NSString *version = [[[NSBundle mainBundle] infoDictionary] objectForKey:@"CFBundleShortVersionString"];

// Build can be any string eg `789` or `8A400`
NSString *build = [[[NSBundle mainBundle] infoDictionary] objectForKey:@"CFBundleVersion"];
```
