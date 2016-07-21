---
layout: post
title: "Good Architecture for iOS App"
date: 2016-07-20T17:17:01+08:00
categories: [Architecture, iOS]
---

This post discuss various architecture designs for an iOS app.

It is an ongoing post which I will update continously.


## Good Architecture

I wrote this post because I was reading
[good iOS application architecture](http://slideslive.com/38897361/good-ios-application-architecture-en)
by Krzysztof Zabłocki 

You might know him from [KZLinkedConsole](https://github.com/krzysztofzablocki/KZLinkedConsole)
his popular Xcode plugin [written in Swift](http://merowing.info/2015/12/writing-xcode-plugin-in-swift/)

Traits of Good Architecture:

- Each object has a clear role
- Ability to follow data flow with ease
- Don't depend on any particular framework
- Limited Dependencies
- Flexible because it is simple, not because it is over abstracted

As Uncle Bob says, a good architecture is one when you look at it, eg an accounting software, it screams ACCOUNTING, not MVC/etc. A good architecture will defer the frameworks it use.


## How you know it is bad?

    # How many lines in your view controllers?
    find . -type f -exec wc -l {} + | sort -n
    
[Dependency Visualizer](https://github.com/PaulTaykalo/objc-dependency-visualizer)


## Patterns

How you use patterns
determines it is good or bad

Singleton
is very common
but easily misused
Eg. Logger - almost every class needs logging, so singleton is fine

But when you have a VC using a Singleton
just so it is convenient
then it is likely wrong

Composition
is 1 pattern that you must know,
which will lead to other great patterns

DI is good
as it encourages reuse,
and even Apple encourages it in WWDC 2016-06


## 1. MVC

Apple's MVC is wrong
because it's View and Controller are too closely coupled

Only model can be reused

MVC is invented in the 80s
but it can still be good
if you do composition

Apple sample code –
even they admit it is to merely to show a technology,
not written in good architecture

Problems with our View Controller:

- Knowing business logic
- Maintaining states


## 2. VIPER

Good
But lots of boilerplate


## 3. MVVM

Most popular iOS Architecture?

View Model
is UIKit independent
and is testable

But no router


## 4. MVVM+

With Flow Coordinators/Controller/Router

[Improving Architecture with Flow Controllers](http://merowing.info/2016/01/improve-your-ios-architecture-with-flowcontrollers/):

- At least 1 root flow controller
- Created by AppDelegate
- Has multiple child controllers (VCs)
- VC/VM does not know about other VC/VM
- Configures and coordinate different screens
- To support multiple devices, subclass the root flow controller
- You can use for MVC or any other architecture

[Coordinator Redux](http://khanlou.com/2015/10/coordinators-redux/) by Khanlou

What's wrong with such a line of code in a view controller?

```swift
[self.navigationController pushViewController:detailViewController animated:YES];  
```

> It’s bossing its parent around. In real life, children should never boss their parents around. In programming, I would argue children shouldn’t even know who their parents are!

Libraries vs frameworks

> They say the distinction between libraries and frameworks is that you call libraries, and frameworks call you. I want to treat 3rd-party dependencies as much like libraries as possible.


A con of using flow coordinators:
It does not play well with segue


## [5. ReSwift](https://github.com/ReSwift/ReSwift)

The new kid in the block, inspired by Redux.
An [introduction presented by Benjamin Encz](https://realm.io/news/benji-encz-unidirectional-data-flow-swift/)

Single point of truth

It can load a state - persisting state across application launches

[![](http://reswift.github.io/ReSwift/master/img/reswift_detail.png)](http://reswift.github.io/ReSwift/master/getting-started-guide.html)


