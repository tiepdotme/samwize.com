---
layout: post
title: "MVVM for iOS & Swift"
date: 2016-07-26T16:14:37+08:00
categories: [Architecture, iOS]
---

Following a [research into better architectures](/2016/07/20/good-architecture-for-ios-app/), this is some learnings from [MVVM in Swift](http://artsy.github.io/blog/2015/09/24/mvvm-in-swift/).

## View Model (VM)

The VM is a 1-1 with view controller.

It is actually simply a composition pattern, by separating responsibility from the View Controller (VC).


## What is View Model responsible for?

View Model stands between the Model and the View Controller, and provide the data that a view controller needs to display in it's views.

View Controller NO longer access the model directly.

If the model has a `NSDate`, then the VM will have the `NSDateFormatter` that knows how to display the date as a string for the view.


## What is View Model NOT responsible for?

A VM does not know about the view.

It does not know stuff about presentation, but it does provide the data for presenting. 

Note that is "data" for presenting is not the actual model, but a intermediatry object (aka boundary object).


## What is grey?

Networking call is a grey area. Neither VM nor VC defines where the network logic should go.

But it is safer to be in VM. Or use another composition pattern with a network object.


## How different is MVVM and MVC

It is actually not much different.

The introduction of a View Model simply extract the business/app logics out of a View Controller.

It uses a composition pattern, so a VC now will have a VM. In doing so, you can now write tests for the VM (which has the important business logics).


## How View Model (VM) communicates with View Controller (VC)?

Remember, VM does not know anything about the VC.

Just like VC does not know anything about the model.

VM and VC can communicate via delegates or Functional Reactive Programming (FRP).

You would have have know delegate pattern, which is widely used in Apple's frameworks. It can work, except it is verbose.

A better way is FRP, which you would have heard in the form of [Reactive](https://github.com/ReactiveKit/ReactiveKit). A good introduction to FRP is the [Swift Bond tutorial by raywenderlich](https://www.raywenderlich.com/123108/bond-tutorial)

(alas, Swift does not support binding out of the box, so a tool like ReactiveKit is much needed)


## ReactiveKit

This very cool framework was first known as [Bond](https://github.com/SwiftBond/Bond), _Swift Bond_.

It's successor is ReactiveKit, which is now made up of a few components:

- [ReactiveKit](https://github.com/ReactiveKit/ReactiveKit) - The core
- [ReactiveUIKit](https://github.com/ReactiveKit/ReactiveUIKit) - provides extension to UIKit to provide binding
- [ReactiveAlamofire](https://github.com/ReactiveKit/ReactiveAlamofire) - provides extension to the popular networking library

Compared to other libraries doing the same think (like Reactive Cocoa), ReactiveKit looks great.
