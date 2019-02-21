---
layout: post
title: "Tools for macOS App Development Is Limited"
date: 2019-02-20T17:20:04+08:00
categories: [macOS]
---

macOS is way older than iOS, but the ecosystem way worse.

First party (Apple) prioritizes iOS over macOS, for obvious popularity reason.

Third party support is even more limited.

## Cocoapods

Cocoapods work for macOS, as long as the pods itself support for macOS.

## fastlane

Officially, they [don't support](https://github.com/fastlane/fastlane/commit/1b85d6f180722bd96a1d0cc8493c6c1bb36f0ce5) macOS.

It started as a half-hearted effort to support all Apple platforms, but eventually decided not worth the resources. But some modules are (luckily/compatibly) working.

So far, these fastlane modules works for me: `bump`, `gym`/`build_app`

Sadly, these don't work: `match`, `deliver`

## Analytics/Crashlytics

Unfortunately, the most popular SDKs on iOS does not build on macOS.

What does NOT work? Firebase, Fabric, Flurry.

Some [alternatives](https://www.quora.com/Is-there-any-good-analytics-platform-that-I-can-use-for-my-Mac-Cocoa-app), but none of which I have tried.

## The Future

Apple has [announced cross-platform support](https://www.theverge.com/2018/6/4/17418994/iphone-app-mac-support-ios-macos-wwdc-2018) for macOS, by porting the millions of iOS apps (almost automatically).

And so, third party libraries will have to play along.

I will wait for Firebase etc to play nice..
