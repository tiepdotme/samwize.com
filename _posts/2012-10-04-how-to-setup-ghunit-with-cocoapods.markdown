---
layout: post
title: "How to setup GHUnit with CocoaPods"
date: 2012-10-04 23:21
comments: true
categories: iOS
---

The [installation guide for GHUnit](http://gabriel.github.com/gh-unit/docs/appledoc_include/guide_install_ios_4.html) is for installing the framework in the traditional way.

As you might know, [CocoaPods](http://cocoapods.org/) is the new way to handle dependencies. Of course, GHUnit is supported. 

The is a lack of a guide on how you setup the two useful iOS libraries together.

<!-- more -->

It is similar to the [official guide](http://gabriel.github.com/gh-unit/docs/appledoc_include/guide_install_ios_4.html). I am going to point out the differences when need to.

## Step 1 - Create Test Target ##

This is the same. You create a completely new application target. 

I prefer to name it `AppGHTests`, as I might still be using `AppTests` for my [SenTestings](/2012/10/03/sentestingkit-does-not-support-wait-for-blocks).

{%img center /images/xcode-ghunit-pods-new-target.png %}



## Step 2 - Configure Test Target ##

You DON'T have to download and copy `GHUnitIOS.framework` to your project since you are using CocoaPods. Instead, you should setup GHUnit pods.

Edit the `Prodfile` and add GHUnitIOS.

	platform :ios
	pod 'GHUnitIOS', '0.5.5'

Then install the pod as per normal.

	$ pod install

Continue with the official guide to remove the unnecessary files, and edit `main.m` to replace the delegate class with `GHUnitIOSAppDelegate`.



## Step 3 - Configure Pod for Test Target ##

This part is IMPORTANT. The new test target will not include the pods. 

You need to configure the target to be based on Pods project. Refer to [this post](http://samwize.com/2012/10/01/unit-tests-with-cocoapods/).

Lastly, add the `libPods.a` library to the test target.

{%img center /images/xcode-ghunit-pods-libpods.png %}

That's it. Run the test target!

