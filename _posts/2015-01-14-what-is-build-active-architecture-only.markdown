---
layout: post
title: "What is Build Active Architecture Only"
date: 2015-01-14 09:18:46 +0800
comments: true
categories: iOS
---

Xcode Build Settings has a `Build Active Architecture Only` for every project/target.

**YES** - If set to yes, then Xcode will detect the device that is connected, and determine the architecture, and build on _just that architecture_ alone.

**NO** - If set to no, then it will build on all the architectures.

There are pros and cons.

If there's no problem, setting to YES will make building faster.

But sometimes, you can't. For example if your project does not support 64-bit (yet), and your device is the latest iPhone 6 which needs 64-bit, then your project will not be able to run. If you change to NO, then the project will build armv7 etc, which will work.
