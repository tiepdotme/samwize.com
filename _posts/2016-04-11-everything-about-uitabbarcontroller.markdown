---
layout: post
title: "Everything About UITabBarController"
date: 2016-04-11T12:46:03+08:00
categories: [iOS]
published: false
---


## Translucent Tab Bar

While [tab bars](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/UIKitUICatalog/UITabBar.html) are default **translucent** in iOS 7, it wasn't obvious what are the steps needed.

This is the code needed, but you could very well configure in interface builder:

```swift
tabBar.translucent = true

// `scrollView` could be tableView/collectionView/etc
// It needs to be NOT clip to bounds so that it shows up behind the bar bar
scrollView.clipsToBounds = false
```
