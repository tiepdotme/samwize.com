---
layout: post
title: "Singleton/SharedInstance Template for iOS 5 (ARC and GCD)"
date: 2012-09-29 19:35
comments: true
categories: iOS
---

The singleton pattern is widely used in iOS to have a global, static class. 

The most famous is `[NSUserDefaults standardUserDefaults]`. 

Over the years, the template for creating a singleton/SharedInstance has also changed. This is due to the introduction of new technologies in Apple's SDK, particularly ARC and GDC.

<!-- more -->

The [implementation](http://lukeredpath.co.uk/blog/a-note-on-objective-c-singletons.html) has simplified to:

``` objc
+ (id)sharedInstance
{
  static dispatch_once_t pred = 0;
  __strong static id _sharedObject = nil;
  dispatch_once(&pred, ^{
    _sharedObject = [[self alloc] init]; // or some other init method
  });
  return _sharedObject;
}
```

Nice piece of code added to my Xcode snippet.