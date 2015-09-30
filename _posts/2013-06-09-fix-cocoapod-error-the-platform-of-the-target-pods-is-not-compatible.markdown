---
layout: post
title: "Fix cocoapod error: the platform of the target pods is not compatible"
date: 2013-06-09 22:46
comments: true
categories: [iOS]
---

While using [Cocoapod](http://cocoapods.org/) and running `pod install`, I encountered the following error:

	[!] The platform of the target `Pods` (iOS 4.3) is not compatible with `FlatUIKit (1.1)` which has a minimum requirement of iOS 5.0.

<!-- more -->

There is obviously a dependency error. Immediately, I open Xcode, go to Project Building Settings, and changed **iOS Deployment Target** to 5.0. I was pretty sure that is the way to change the minimum deployment version.

However, running the command gives me the same error.

I was puzzled. Did I set it wrongly in Xcode? I started to question myself..

After some probing, I realized it is Cocoapod's [fault](https://github.com/CocoaPods/CocoaPods/issues/603).

You have to write in `Podfile` your minimum deployment version eg.

```
platform :ios, '5.0'
pod 'FlatUIKit', '~> 1.1'
```

If you don't specify '5.0', it is taken to be '4.3'. Duh.