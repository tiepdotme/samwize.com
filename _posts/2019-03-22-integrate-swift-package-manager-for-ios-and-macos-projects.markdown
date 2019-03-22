---
layout: post
title: "Integrate Swift Package Manager for iOS & macOS Projects"
date: 2019-03-22T15:21:51+08:00
categories: [Swift]
---

Swift Package Manager (SPM) [does not support iOS and macOS](https://github.com/apple/swift-package-manager/blob/master/Documentation/Usage.md#depending-on-apple-modules). Sadly, it has been a few years and they are still not on the same level as CocoaPods.

_What then is SPM good for?_ It is good for developing executable CLI or packing your library.. not sexy for the modern app developer.

## But it should work

SPM is nothing but resolving the dependencies, downloading them as needed.

Hence, it should work even if we drag and drop the frameworks into a project.

I will describe how I do so with my specific scenario: using [MongoKitten](https://github.com/OpenKitten/MongoKitten) for my macOS app.

The same steps will apply for any libraries, and for iOS (or even tvOS, watchOS too).

## Why did I not use CocoaPods ?

Because, MongoKitten podspec did not support macOS, for some reason. But it will work since the library does not use any specific iOS frameworks.

If CocoaPods works for you, please do not use CPM.

## The steps

Before I describe the steps, read [here](/2016/11/14/swift-package-manager-development-guide/) for a basic guide on using `swift package` commands.

### 1. Create The Dependencies

We will setup the dependencies that will be resolved by SPM in your app root directory.

    # In the root folder of eg. MyApp.xcodeproj
    mkdir Dependencies
    cd Dependencies
    swift package init

Edit `Package.swift` for the actual libraries you need (eg. MongoKitten):

```swift
// swift-tools-version:4.2

import PackageDescription

let package = Package(
    name: "Dependencies",
    products: [
        .library(name: "Dependencies", type: .static, targets: ["Dependencies"])
    ],
    dependencies: [
        // Dependencies declare other packages that this package depends on.
        .package(url: "https://github.com/OpenKitten/MongoKitten.git", .exact("4.1.3"))
    ],

    targets: [
        .target(
            name: "Dependencies",
            dependencies: ["MongoKitten"]),
        .testTarget(
            name: "DependenciesTests",
            dependencies: ["Dependencies"]),
    ]
)
```

Now build them:

    swift build

### 2. Generate xcodeproj

Simply run the command:

    swift package generate-xcodeproj

I have seen on [stackoverflow](https://stackoverflow.com/a/54701806/242682) that declared custom xcconfig, then generate with
`swift package generate-xcodeproj --xcconfig-overrides macos.xcconfig`. I did not need it, but it's there if things didn't work on your first try.

### 3. Add Dependencies.xcodeproj to Your App

Now, open **your app's** Xcode project/workspace.

Drag and drop the generated Dependencies.xcodeproj to the Project Navigator.

![](/images/spm-dependencies.jpg)

Next, go to your app target > Linked Frameworks and Libraries > Add Dependencies.framework

![](/images/spm-dependencies-linked.jpg)

This will ensure the Dependencies (the libraries) will be build first before the app.

With that, you're done. Go ahead and import any of the libraries (eg. MongoKitten) in the app.

_For MongoKitten, you make sure you enable Outgoing Connections in App Sandbox entitlements._

### Updating dependencies

You have to run these 2 commands manually whenever you choose to update:

    swift package update
    swift package generate-xcodeproj
