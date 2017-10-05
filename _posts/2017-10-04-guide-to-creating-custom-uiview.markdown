---
layout: post
title: "Guide to Creating Custom UIView"
date: 2017-10-04T10:42:21+08:00
categories: [UI]
---



## About Initializer

It is important to learn the fundamental about [Swift initialization](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Initialization.html). It is a complex and lengthy topic, but will be useful in understanding as we subclass `UIView` later.

We keep a short version here anyway.

`required` and `convenience` are both used to specify restrictions on subclasses.

`convenience` is secondary/optional, and they are simply shortcut to calling designated initializer.

How do you specify designated initializer?

You _don't_ specify `convenience`, which means simply `init(){ ... }`

Subclass MUST call it's superclass designated initializer -- not difficult to reason because without so, the superclass would not be completely initialized.




## Intrinsic Content Size

A custom view should never specify constraints for it's width and height. But if it wants to, it should specify in the 

https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/AnatomyofaConstraint.html#//apple_ref/doc/uid/TP40010853-CH9-SW21

Cook book
https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/ViewswithIntrinsicContentSize.html