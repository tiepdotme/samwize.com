---
layout: post
title: "Ideal Xcode Project Structure"
date: 2020-06-30T15:22:35+08:00
categories: [Xcode]
---

The [Fruta sample code](https://developer.apple.com/documentation/swiftui/fruta_building_a_feature-rich_app_with_swiftui) provides a good example of a project structure that supports multi-platforms.

## Use Group as Folder

Every group in Xcode is an actual folder.

_Stop using logical groups without folder._

## The Main Groups

- Shared
- iOS
- iOS Clip
- iOS Widgets
- macOS
- macOS Widgets
- Packages
- Playgrounds

Platforms & extensions have their own group. Within them, structure as per your selected architecture eg. MVVM, VIPER. Or any logical grouping that makes the most sense.

## Shared Code

The `Shared` group is for code used in **all** platforms. In Fruta, almost all the code is under `Shared`. Only few Swift files are in the platform specific group.

Even the `@main App` is in Shared, and it runs for all: iOS, macOS, widgets, clips. It works because it uses preprocessor code. Alternatively, we could create specific Swift file for each platform.

You can even breakdown further eg. `Shared-iOS`. It is up to you as needed.

## Packages

Local packages within the projects. Eg. `Fruta-Networking`. This is also shared code, but in a formal self contained package. Later on, they can also be moved out of the project to become an external dependency.

These packages are selectively added to the targets; they can be excluded in a certain target.

## Use of Preprocessor Code

Aka _Active Compilation Conditions_ (NEW!) under build settings. Yet not exactly new, since before this, we already have been adding to [Other Swift Flags](/2019/02/26/preprocessor-codes-for-swift/).

With that, we can write preprocessors such as `#if APPCLIP`, or `#if ENABLE_DANCE_MONKEY_FEATURE`.

We can also use `#if os(iOS)` etc.

## Playgrounds

Playground is a scratchpad. Nice to try out some codes, or even to explain certain concepts.

## Tests

The Fruta project did not tests. Generally, you have 2 types of tests:

1. Unit Testing
2. UI Testing

Therefore each platform will have to create 2 test targets eg. `FrutaiOSUnitTests`, `FrutaiOSUITests`.

That's a lot of tests, if you're testing 😄
