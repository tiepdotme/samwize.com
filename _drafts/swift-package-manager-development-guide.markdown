---
layout: post
title: "Swift Package Manager Development Guide"
date: 2016-10-29T07:44:51+08:00
categories: [Swift]
---

[Previously](http://samwize.com/2016/05/20/introduction-to-scripting-in-swift/), we mentioned a Rome version of Cocoapods that you can use for your Swift scripts.

Swift is moving fast, and now we have the official [Swift Package Manager](https://swift.org/package-manager/)!.

Instead of writing a `Podfile`, you write a `Package.swift`, which is just Swift in it.

Then to get the dependency, just `swift build` as normal.


## More Commands 

    # If you prefer to setup your with more templates, including .gitignore, Sources and Tests
    swift package init

    # Generate the Xcode Project  
    swift package generate-xcodeproj

    # Update
    swift package update

    # Show the dependency as a tree
    swift package show-dependencies