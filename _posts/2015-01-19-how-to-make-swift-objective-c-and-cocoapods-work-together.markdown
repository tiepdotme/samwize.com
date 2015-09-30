---
layout: post
title: "How to make Swift, Objective-C and Cocoapods work together"
date: 2015-01-19 16:43:28 +0800
comments: true
categories: [iOS, Swift]
---

[Interoperability](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/BuildingCocoaApps/InteractingWithObjective-CAPIs.html) is possible with Swift and Objective-C.

Apple has a guide on how to make the same project to [use both the languages](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/BuildingCocoaApps/MixandMatch.html#//apple_ref/doc/uid/TP40014216-CH10-XID_77). The gist is to create a new bridging header.

<!-- more -->


# Add a bridging header

There's a [step-by-step answer](http://stackoverflow.com/a/24005242/242682) to creating the bridging header.

Summary of my steps:

1. Create a new Objective-C header file eg. `tmp.h` in your Watchkit Extension

2. When prompted, let Xcode automatically configure the project

    {%img http://i.stack.imgur.com/nakLZ.png %}

3. Import your objective-c lib headers in the automatically created bridging header file

With that, you can use the Objective-C code in your Swift code.

You may also delete `tmp.h`.


# Making it work with Cocoapods

At this point, if you are using cocoapods, you will encounter `file not found` on the import statement.

This is because [Cocoapods is not configured for the Watch app extension](http://stackoverflow.com/a/25555476/242682).

To fix, you click on Project > Info > Configurations > expand the dropdown, and select the corresponding Pods just like how it is for the main app.

