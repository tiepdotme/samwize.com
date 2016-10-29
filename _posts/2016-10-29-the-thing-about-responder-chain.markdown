---
layout: post
title: "The Thing About Responder Chain"
date: 2016-10-29T11:44:02+08:00
categories: [Swift]
---

## Who are Responders?

- `UIView` is `UIResponder` type
- `UIViewController` is also `UIResponder` type

They are the common classes that deals with responding to touches, motion events and etcs.


## Who is the FIRST responder?

When user tap on a view, the system has to first find out _who is the first responder?_

This is known as a **hit-test phase**.

A hit testing phase starts from the lowest level (the window). 

_Hit test traverse up, while responder chain traverse down._

`hitTest:withEvent:` will be called [recursively](http://stackoverflow.com/a/4961484/242682) into subviews (traverse up), until it reaches the **leaf view**. That responder is the first.


## The Chain

After identifying the first responder, then the responder chain begins, starting with the first responder of course.

![Responder Chain](https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/Art/iOS_responder_chain_2x.png)

There are rules to finding the next responder.

1. The first responder passes the event to its view controller if it has one; if not, it passes to its superview.
2. If a view or its view controller cannot handle the event, it passes to the next responder (superview or parent view controller).
3. Each subsequent superview in the hierarchy follows the pattern described in the first two steps if it cannot handle the event or message.
4. Topmost view in the view hierarchy, or next
5. UIWindow, or next
6. Lastly, App Delegate


## The target-action trick 

You can set a target to nil, and it will traverse up the responder chain to find the action.

An example from [swiftandpainless.com](http://swiftandpainless.com/utilize-the-responder-chain-for-target-action/).

