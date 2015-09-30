---
layout: post
title: "Why I am not going to use ReactiveCocoa"
date: 2014-03-13 16:19:37 +0800
comments: true
categories: iOS
---

[ReactiveCocoa](https://github.com/ReactiveCocoa/ReactiveCocoa) is a framework for functional reactive programming.

In short, it is a framework to unify [delegate patterns](https://developer.apple.com/library/ios/documentation/general/conceptual/DevPedia-CocoaCore/Delegation.html), KVO notifications, and other callback patterns.

Ray Wenderlich has covered with a [2-parts](http://www.raywenderlich.com/62699/reactivecocoa-tutorial-pt1) [tutorial](http://www.raywenderlich.com/62796/reactivecocoa-tutorial-pt2).

So why am I not going to use the framework?

<!-- more -->

I concur with the "final thoughts" from [bignerdranch.com](http://blog.bignerdranch.com/4549-data-driven-ios-development-reactivecocoa/).

### 1. Unpleasing Syntax

I don't like the syntax such as `RACObserve(self, username)`, `[RACSignal combineLatest:...]` or `self.button.rac_command = ...`.

It could be a personal thing, but I just don't like it.


### 2. Cannot replace entirely

ReactiveCocoa will not be able to replace UITableView's datasource and table delegate pattern. There's many things it cannot replace.

Sadly, it will not be able to unify the different patterns.

It will have to continuously implement category for UIButton (to have `self.button.rac_command`) and etc.


### 3. When there is bug..

When there is bug, you have 1 more framework to look into.


### 4. When there is performance hit..

When you need to improve performance, this framework could be hindering.

I have a suspicion: The `filter` method is kind of unnecessary. Who knows what else/magic they do behind observing all the signals, filtering, then passing you the signals?

Using the fameworks directly is clearer for me.


### 5. I can live without it

I have no problem using UITextField delegate or [LocationManager](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html). I don't need another framework to go along with it.

This jump is not revolutionary enough. I will wait for something else better that could come along later.

Could be even from Apple. Blocks are here, and that is good start to solving spaghetti code.

