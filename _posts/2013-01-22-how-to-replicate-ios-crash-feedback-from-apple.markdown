---
layout: post
title: "How to replicate iOS Crash feedback from Apple"
date: 2013-01-22 00:00
comments: true
categories: iOS
---

If you submit an iOS app to Apple, and they get back to you with the unfortunate news that your app crashed.. you have to (of course) fix the bug and resubmit again.

To help you, Apple engineers will

- explain how they did it, 
- attach a screenshot, and
- attach a crashlog

This is what you should do.

<!-- more -->

Open Xcode > Organizer > Devices > Library > Device Logs > select **Import** at the bottom to open the crashlog file. This will [symobolicate](https://developer.apple.com/library/ios/#technotes/tn2008/tn2151.html) the crash report and let you know which line of code failed.

If you still do not understand why, you could install the same IPA into your device.

To do so, in Xcode > Organizer > Archives > Your app > Distribute > [Save for Enterprise or Ad-Hoc Deployment](http://developer.apple.com/library/ios/#qa/qa1764/_index.html), double click and sync with iTunes. Your device will now run the same copy that you submitted to Apple.

Hopefully, you can then squash the bug.