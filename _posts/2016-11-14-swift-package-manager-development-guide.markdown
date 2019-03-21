---
layout: post
title: "Swift Package Manager Development Guide"
date: 2016-11-14T07:44:51+08:00
categories: [Swift]
---

[Previously](http://samwize.com/2016/05/20/introduction-to-scripting-in-swift/), we mentioned a Rome version of Cocoapods that you can use for your Swift scripts.

Swift is moving fast, and now we have the official [Swift Package Manager](https://swift.org/package-manager/)!.

Instead of writing a `Podfile`, you write a `Package.swift`, which is made up of Swift code declaring the dependencies.

Then to get the dependencies:

    swift build

## More Commands

    # If you prefer to setup with a template, including .gitignore, Sources and Tests
    swift package init

    # Generate the Xcode Project
    swift package generate-xcodeproj

    # Update
    swift package update

    # Show the dependency as a tree
    swift package show-dependencies

I want to highlight the use of `swift package generate-xcodeproj`. This is needed so that you can write your code with an IDE. _Who doesn't love code completion._

There was a time where my generated Xcode project broke (could be due to upgrade of Xcode, or Swift Package Manager). The fix was to rerun the command.

They are constantly upgrading, and there are bounds to be changes. You will need these docs too:

- [Usage](https://github.com/apple/swift-package-manager/blob/master/Documentation/Usage.md)
- Describe the [package format](https://github.com/apple/swift-package-manager/blob/master/Documentation/PackageDescriptionV4.md), especially the dependecies versions.
