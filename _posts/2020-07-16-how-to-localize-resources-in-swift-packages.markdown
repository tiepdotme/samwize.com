---
layout: post
title: "How to Localize Resources in Swift Packages"
date: 2020-07-16T15:52:09+08:00
categories: [SPM]
---

At last, in Swift 5.3, the updated Swift Package Manager (SPM) now supports resources! Yipee~

This post is a guide to adding resources to your package.

## 1. Add to Target

The convention is to add them in a "Resources" subfolder for the **target**. So if you have a target called `MyTarget`, you will add resources to

> `/Sources/MyTarget/Resources/`

But you could actually add to _anywhere_ in "MyTarget".

## 2. Edit Package.swift

Add `defaultLocalization` to the **package** descriptor.

```swift
let package = Package(
    name: "MyPackage",
    defaultLocalization: "en",
    ...)
```

## 3. Implicit & Explicit Resources

Xcode automatically recognize for these **known resources**:

- XIB, storyboards
- Core Data xcdatamodeld
- Asset Catalogs
- strings files
- `.lproj`

So for the above known resources, there is nothing else to do.

For for other resources, you need to declare them explicitly.

```swift
targets: [
    .target(
        name: "MyLibrary",
        resources: [
            .process("flu.jpg"),
            .copy("pandemic.json") // Do NOT process
            ]
    ),
]
```

`process` will have Xcode optimize the resource for the platform, while `copy` will not.

## 4. Expose in the package

You should **expose resources from within the package**, instead of having the app accessing them directly. So let's say you have this localized string "Okay", you will publicly expose with

```swift
public let localizedOkay = NSLocalizedString("Okay", bundle: Bundle.module, comment: "")
```

There is something new here: `Bundle.module`

This is a generated code for the package (specifically the target)! Prior to this, we make use of `Bundle(name:inFramework:)`.

You cannot (and it does not make sense) to use `Bundle.module` in an app. Though it doesn't stop you from exposing it with

```swift
public let bundleForMyTarget = Bundle.module
```

Then in the app, you can use `NSLocalizedString("Okay", bundle: bundleForMyTarget, comment: "")`. _Just suggesting._

## PITFALL: App must add localization

Even if a package supports a certain localization, the app has to add it first under the app's Project > Localizations.

It seems like Xcode will optimize and strip out localizations, if they are not supported in the app.
