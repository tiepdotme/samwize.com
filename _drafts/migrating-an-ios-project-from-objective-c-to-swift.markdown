---
layout: post
title: "Migrating an iOS Project From Objective-C to Swift"
date: 2016-11-22T23:09:55+08:00
categories: [Swift]
---

# UIApplicationDelegate

Start with the app delegate.

Add a `AppDelegate.swift`, and mark the class with `@UIApplicationMain` attribute.

Remove `main.m`, because it is now unnecessary. With the use of `@UIApplicationMain`, the target now knows the entry point.


# Adding your first Swift

When you add your first Swfit file (like `AppDelegate.swift` above), you will notice that Xcode will prompt to add a bridging header.

Add the bridging header.

This `AwesomeApp-Bridging-Header.h` is the place where you add your .h files, exposing them to Swift.


## Using Objective-C in Swift

We come to the first of the 2 important parts in interoperability. 

To use your existing Objective-C, there are a few things to do:

1. Bridging header to expose the Objective-C classes 
2. Declare nullability in the Objective-C files

We have mentioned (1) in the section above.

For (2), the reason is because Swift is strict on nullability. Can an object be optional, or never?

In Objective-C days, way before Swfit was born, nullability was never explicit. You would have seen `EXEC_BAD_ACCESS` and many times because the object is nil, when is should not be.

Swift make our code safer with explicit nullability.

Hence, you need to make the Objective-C code be explicit too:

- 


## Using Swift in Objective-C

This is the second part.
