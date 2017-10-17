---
layout: post
title: "Guide to Creating Custom UIView"
date: 2017-10-04T10:42:21+08:00
categories: [UI]
published: false
---

This is a guide to creating custom `UIView` using Auto Layout, without Nib/Storyboard, and in Swift 4.

## Why custom `UIView`?

We create custom view when the controls from UIKit is not sufficient to provide.

The custom view will be made up of other views, with certain custom behaviours.

There are often times when you construct your storyboard or init your view controller with multiple views, but they could be accomplish with a custom view.

Let's look at an example.

UIImageView + UILabel in a UIStack

A custom view will be an AppIconView. Init with image and text.

## About Initializers

It is important to learn the fundamental about [Swift initialization](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Initialization.html). It is a complex and lengthy topic, but will be useful in understanding because our custom view subclass `UIView`.

### Designated initializer

You _don't_ specify `convenience`, it is default, which means simply `init(){ ... }`

Subclass MUST call it's superclass designated initializer -- not difficult to reason because without so, the superclass would not be completely initialized.

### Convenience initializer

`convenience` is secondary/optional, and is simply shortcut to calling designated initializer.

### Required init

`required` specify that subclasses must implement the initialization. `UIView` has such an init because it conforms to [`NSCoding`](https://developer.apple.com/documentation/foundation/nscoding), a protocol for the view to be encoded and decoded for archiving. 

Our custom view has to implement (it is an override, but without the override modifier), and decode to init the view.

```swift
public required init?(coder aDecoder: NSCoder) {
  super.init(coder: aDecoder)
  // Custom decoding..
}
```

But, you probably will not have any custom decoding, because archiving a view is a bad idea (instead you should archive the model). 

The corresponding func is [encoding](https://developer.apple.com/documentation/foundation/nscoding/1413933-encode).

### Custom view initializers

...

http://www.edwardhuynh.com/blog/2015/02/16/swift-initializer-confusion/

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


## IBDesignable 

