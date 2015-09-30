---
layout: post
title: "Unit Tests with CocoaPods"
date: 2012-10-01 23:41
comments: true
categories: [iOS, OSX, Testing]
---

[CocoaPods](http://cocoapods.org/) is a tool for managing dependecies for iOS and Mac OS X projects. 

[Getting started](http://cocoapods.org/) with using CocoaPods is easy. The tricky part is getting used to opening Xcode workspace (eg. `App.xcworkspace`) instead of Xocde project. But that's not what I want to covered in this post.

One of the hurdle I encountered is getting unit testing to work after CocoaPods is setup.

<!-- more -->

Apparently, after you setup CocoaPods, it automatically configures your project's main target to use the pods (dependecies). However, it does not configure the same for your unit tests target. So when you run your unit test, you will likely encounter errors like this:

	SomeLibrary.h not found

To fix, go to **Project** > **Info** > **Configurations** > change `Based on Configuration File` to `Pods`. Like this:

{%img center /images/cocoapods-unit-test-configure.png %}