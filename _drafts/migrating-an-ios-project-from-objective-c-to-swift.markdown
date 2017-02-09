---
layout: post
title: "Migrating an iOS Project From Objective-C to Swift"
date: 2017-02-10T19:09:55+08:00
categories: [Swift]
---

# UIApplicationDelegate

Start with the app delegate.

Add a `AppDelegate.swift`, and mark the class with `@UIApplicationMain` attribute.

Remove `main.m`, because it is now unnecessary. With the use of `@UIApplicationMain`, the target now knows the entry point.


# Adding your first Swift

When you add your first Swift file (like `AppDelegate.swift` above), Xcode will prompt to add a bridging header.

Add the bridging header.

The `AwesomeApp-Bridging-Header.h` is the place where you add your existing .h files, exposing them to Swift (as and when needed).


## 1. Using Objective-C in Swift

We come to the first of the 2 important parts in [interoperability](https://developer.apple.com/library/content/documentation/Swift/Conceptual/BuildingCocoaApps/InteractingWithObjective-CAPIs.html). 

To use your existing Objective-C, there are a few things to do:

1. Bridging header to expose the Objective-C classes 
2. Declare nullability in the Objective-C files

We have mentioned (1) in the section above.

For (2), the reason is because Swift is strict on [nullability](https://developer.apple.com/library/content/documentation/Swift/Conceptual/BuildingCocoaApps/InteractingWithObjective-CAPIs.html#//apple_ref/doc/uid/TP40014216-CH4-ID45). Can an object be optional, or never?

In Objective-C days, way before Swfit was born, nullability was never explicit. You would have seen `EXEC_BAD_ACCESS`, and many times the reason is because the object is nil, when is should not be.

Swift make our code safer with explicit nullability.

Hence, you need to make Objective-C code be explicit, so that Swift can use safely:

- `nullable` - An optional
- `nonnull` - Never an optional
- Otherwise, they are implicitly unwrapped optional (crash if nil!)

There are also 2 convenient macros to help you: `NS_ASSUME_NONNULL_BEGIN` and `NS_ASSUME_NONNULL_END`. Any properties between the macros are considered `nonnull`.


## 2. Using Swift in Objective-C

This is the second part, if you still need to write _more_ Objective-C.

To use the new Swift code in existing Objective-C, you have to 

    #import "AwesomeApp-Swift.h"
    
This will expose all Swift code.

Don't be alarm that you cannot find the aforementioned .h file in your project. It is a auto generated header from your Swift code.
