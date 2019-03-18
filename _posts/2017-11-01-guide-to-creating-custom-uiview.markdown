---
layout: post
title: "Guide to Creating Custom UIView"
date: 2017-11-01T10:42:21+08:00
categories: [UI]
---

This is a guide to creating custom `UIView` using Auto Layout, without Nib/Storyboard, and in Swift 4.

## Why custom UIView?

We create custom view when the controls from UIKit is not sufficient to do your job.

Custom view will be composed of other views, with certain custom behaviours.

There are often times when you construct your storyboard or init your view controller with multiple views, but they could -- alternatively -- be accomplished with a custom view.

## About Initializers

It is important to learn the fundamental about [Swift initialization](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Initialization.html). It is a complex and lengthy topic, but will be useful in understanding because our custom view subclass `UIView`.

### Designated initializer

You specify a designated initializer by _not_ specifying `convenience`.

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

## Our custom view initializers

With the basics of initializers explained, a custom view will usually need a few inits like this:

```swift
public class AppIconView: UIView {

    // #1
    public required init?(coder aDecoder: NSCoder) {
        super.init(coder: aDecoder)
        setupView()
    }

    // #2
    public override init(frame: CGRect) {
        super.init(frame: frame)
        setupView()
    }

    // #3
    public convenience init(image: UIImage, title: String) {
        self.init(frame: .zero)
        self.image = image
        self.title = title
    }

    private func setupView() {
        translatesAutoresizingMaskIntoConstraints = false

        // Create, add and layout the children views ..
    }

}
```

There are 3 inits and here is why:

1. Because `UIView` **required** it
2. Because #3 will need to call a designated initializer (we choose #2 over #1)
3. Our own initializer with the data for the view

In each of the init, `setupView()` will run -- which is the one place where we will configure and layout the custom view.

### translatesAutoresizingMaskIntoConstraints

Every view that uses auto layout should set `translatesAutoresizingMaskIntoConstraints` to false. That is the very first thing to do in `setupView()`

This is because in days before we have Auto Layout, there is the concept of auto resizing. Via `autoresizingMask`, a view will auto resize, similar to auto layout.

But we don't use auto resize anymore, now that we have auto layout.

### Intrinsic Content Size

The **natural size** for it's content eg. `UILabel` intrinsic size is the length of the text.

Apple's [auto layout guide](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/AnatomyofaConstraint.html#//apple_ref/doc/uid/TP40010853-CH9-SW21) has a section on it, and a [cook book](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/ViewswithIntrinsicContentSize.html) of common scenarios.

How does it work?

> It generates a pair of constraints for each dimension (width & height). The pair constrain the compression resistance (priority 750) and content hugging (priority 250).

Intrinsic content size simplifies auto layout, reducing number of constraints needed, because those constraints are added for you. You only need to manage the priority.

It is a helper, and you ought to understand that it adds constraints for you.

Let's say we want our custom view to have a fixed height, but varying width, we can override the `intrinsicContentSize` like this.

```swift
public override var intrinsicContentSize: CGSize {
    let height = ... // Your calculated or fixed height
    return CGSize(width: UIViewNoIntrinsicMetric, height: height)
}
```

As a rule of thumb: **custom view should never create constraint for it's own width and height** (obviously also not for it's postion x & y).

But sometimes you want to _conveniently constraint your size_. This is where intrinsic content size comes in.

### Fitting Size

**Intrinsic size is an input** to Auto Layout engine (which in turn generates/output constraints about it's size).

**Fitting size is an output** from Auto Layout engine.

Fitting size is the size calculated to fit the content.

Use [`systemLayoutSizeFitting(_:)`](https://developer.apple.com/documentation/uikit/uiview/1622624-systemlayoutsizefitting). The parameter `targetSize` is the smallest or largest size that meets the constraint.

Eg. To know the smallest possible size of our content view, call `appView.systemLayoutSizeFitting(UILayoutFittingCompressedSize)`

### The thing with UIStackView

It is easily misled to think stack view has intrinsic size. Using a stack view, it seems the width and height constraints are not required.

But NO, stack view intrinsic size is always `UIViewNoIntrinsicMetric`.

What really happened is that Auto Layout engine calculate the fitting size (an output) for the stack.

So how do you get the fitting size?

Simply `stack.systemLayoutSizeFitting(UILayoutFittingCompressedSize)`.
