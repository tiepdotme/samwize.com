---
layout: post
title: "Out of Memory Crashes"
date: 2016-08-22T10:29:21+08:00
categories: [iOS]
---

Crashlytics has released a new feature to [report out of memory (OOM) crashes](http://crashlytics.com/blog/introducing-oom-reporting).

We all know regular crashes - termination of an app due to some code (app or system libraries).

OOM is another type of crash, which has been _ignored_ in Crashlytics reports until now.

There are 2 types of OOM crashes:

1. Foreground OOM (FOOM) - this crashes like regular crashes
2. Background OOM (BOOM) - app is evicted from iOS

The method to detect OOM is first introducted by Facebook, using a [process of elimination](https://code.facebook.com/posts/1146930688654547/reducing-fooms-in-the-facebook-ios-app/).

![Why is the app starting?](/images/oom-crashes-why-app-is-launching.jpg)

## Crashlytics OOMs

Crashlytics has provided reporting on OOM-free sessions.

Specifically, that is a percentage of sessions that are crash free from FOOM. Note: This is only for **FOOM (Foreground OOM)**, since FOOM are similar to regular crashes while the app is in the foreground.

Also, the number of sessions in Answers does NOT include OOM sessions that crashed.

Crashlytics thrives in providing analytics to crashes, and the inclusion of OOM crashes will be very useful.

Some [pointers](https://docs.fabric.io/apple/crashlytics/OOMs.html) on debugging and fixing these memory issues is provided.
