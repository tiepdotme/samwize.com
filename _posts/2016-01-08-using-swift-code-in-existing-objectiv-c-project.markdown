---
layout: post
title: "Using Swift Code in Existing Objective-C Project"
date: 2016-01-08T18:45:15+08:00
categories: [iOS]
---

While Apple provides [mix-and-match](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/BuildingCocoaApps/MixandMatch.html#//apple_ref/doc/uid/TP40014216-CH10-ID122) between the 2 languages, it is not always possible that code can always be used between the 2.

I have a framework written in Swift, and the method is as such:

```swift
class func findAll(fetchLimit: Int? = nil) {...}
```

It turned out this is not possible to be used in Objective-C.

When I looked under the auto-generated swift umbrella header (MyModule-Swift.h), this method will be omitted.

This is because of the Optional Int. It is a feature of Swift, and not supported by Objective-C!

Just something this simpler and Swift code can't be used.
