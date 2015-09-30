---
layout: post
title: "How to really clean your Xcode build"
date: 2015-07-10 14:43:38 +0800
comments: true
categories: [iOS]
---

There are times when you have weird Xcode build errors eg. you are absolutely sure _that the referenced object_ is in the project, yet Xcode could not find it.

Or compile errors such as `Include of non-modular header inside framework module`, when you know the framework works very fine.

You can try clean (Cmd + Shift + K) and build.

But the **real way to clean Xcode** is to delete folders in `~/Library/Developer/Xcode/DerivedData`. Delete folders with your app name, and also `ModuleCache`. That could well save your day.
