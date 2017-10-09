---
layout: post
title: "Guide to Creating Custom UIView"
date: 2017-10-04T10:42:21+08:00
categories: [UI]
published: false
---

TBC with concise subtopics

## About Initializer

It is important to learn the fundamental about [Swift initialization](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Initialization.html). It is a complex and lengthy topic, but will be useful in understanding as we subclass `UIView` later.

We keep a short version here anyway.

`required` and `convenience` are both used to specify restrictions on subclasses.

`convenience` is secondary/optional, and they are simply shortcut to calling designated initializer.

How do you specify designated initializer?

You _don't_ specify `convenience`, which means simply `init(){ ... }`

Subclass MUST call it's superclass designated initializer -- not difficult to reason because without so, the superclass would not be completely initialized.

## translatesAutoresizingMaskIntoConstraints

Every view that uses auto layout should set `translatesAutoresizingMaskIntoConstraints` to false.

This is because in days before we have Auto Layout, there is the concept of auto resizing. Via `autoresizingMask`, a view will auto resize, similar to auto layout.

We don't use auto resize, now that we have auto layout.

## Intrinsic Content Size

The **natural size** for it's content eg. `UILabel` intrinsic size is the length of the text.

https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/AnatomyofaConstraint.html#//apple_ref/doc/uid/TP40010853-CH9-SW21

Cook book
https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/ViewswithIntrinsicContentSize.html

How does it work?

> It generates a pair of constraints for each dimension (width & height). The pair constrain the compression resistance (priority 750) and content hugging (priority 250).

Intrinsic content size simplifies auto layout, reducing number of constraints needed, because those constraints are added for you. You only need to manage the priority.

## Fitting Size

Intrinsic size is an input to Auto Layout engine (which in turn generates/output constraints about it's size).

Fitting size is an output from Auto Layout engine.

Fitting size is the size calculated to fit the content.

## The thing with UIStackView

It is easily misled to think stack view has intrinsic size. When creating stack view, the width and height constraints are not required.

But NO, stack view intrinsic size is always `UIViewNoIntrinsicMetric`.

What really happened is that Auto Layout engine calculate the fitting size (an output) for the stack.

So how do you get the fitting size?

Use [`systemLayoutSizeFitting(_:)`](https://developer.apple.com/documentation/uikit/uiview/1622624-systemlayoutsizefitting). The parameter `targetSize` is the smallest or largest size that meets the constraint. Example:

```swift
stack.systemLayoutSizeFitting(UILayoutFittingCompressedSize)
```

## Custom View should not specify it's own width and height

Custom views provide constraints for it's subviews, but never for it's own. 

It doesn't make sense to specify your own position.

But sometimes you want to constraint your size. This is where intrinsic content size comes in.

If any dimension has no intrinsic size, then use `UIViewNoIntrinsicMetric`.
