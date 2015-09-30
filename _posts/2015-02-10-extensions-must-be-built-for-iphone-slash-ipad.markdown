---
layout: post
title: "Pitfall: Extensions MUST be built for iPhone/iPad"
date: 2015-02-10 10:55:04 +0800
comments: true
categories: [iOS]
---

I was integrating the new feature of iOS 8 - Today Widget Extension - into one of my app, but got rejected by the Apple review team.

I was told that the Today content is not displayed. A bug?

If it is a bug, I could not replicate it. Therefore I kindly asked for more details from the reviewer.

<!-- more -->

He/She responded that the Today extension worked on iPhone, but not on iPad. And added that *Per the App Extension Programming Guide all extensions are universal, even if the containing app is not.*

I worked towards solving the "bug" by [reading up](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionCreation.html) carefully.

And I found it.

You have to set the extension target build setting **Targeted Device Family** to **iPhone/iPad**.

The default was **iPhone** and that's not good enough.
