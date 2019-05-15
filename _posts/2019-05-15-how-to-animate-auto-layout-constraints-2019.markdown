---
layout: post
title: "How to Animate Auto Layout Constraints (2019)"
date: 2019-05-15T15:15:41+08:00
categories: [iOS]
---

I have a [2014 post in Objective-C](/2014/07/10/autolayout-constraints-animation/), and quite a bit has changed since then. We now have **Swift 5**, and **layout anchors** (a short introduction below).

A comprehensive tutorial comes from [theswiftdev](https://theswiftdev.com/2018/06/14/mastering-ios-auto-layout-anchors-programmatically-from-swift/), ranging from where to create the constraints, including how to animate them.

_However, I find his code on animating constraints differ slightly from my style._

## How I animate constraints

Similar to how I do it in [2014](/2014/07/10/autolayout-constraints-animation/), I change the constraints **within** the animation block.

```swift
UIView.animate(withDuration: 0.25) {
    // Make all constraint changes here
    // For example:
    someViewConstraint.constant = 80
    self.view.layoutIfNeeded()
}
```

[theswiftdev's gist](https://gitlab.com/theswiftdev/uikit/snippets/1741461) does it **before** the animation block. It also works, but that is simply due to the way layout engine process the changes.

There are times you might even have to call `view.layoutIfNeeded()` before the animation, to make sure the animation starts at the right place.

Changing your final constraints within the animation block will ensure that is the final state.

## Layout Anchor

[Layout anchor](https://developer.apple.com/documentation/uikit/nslayoutanchor) was introduced in iOS 9, making the APIs for auto layout much nicer (to code with).

> The NSLayoutAnchor class is a factory class for creating NSLayoutConstraint objects using a fluent API. Use these constraints to programatically define your layout using Auto Layout.

A simple example of setting up a `subview` to have the same _safe_ edges as it's superview:

```swift
view.addSubview(subview)
NSLayoutConstraint.activate([
    subview.leftAnchor.constraint(equalTo: view.safeAreaLayoutGuide.leftAnchor),
    subview.rightAnchor.constraint(equalTo: view.safeAreaLayoutGuide.rightAnchor),
    subview.topAnchor.constraint(equalTo: view.safeAreaLayoutGuide.topAnchor),
    subview.bottomAnchor.constraint(equalTo: view.safeAreaLayoutGuide.bottomAnchor),
])
```

It is much shorter than init-ing each `NSLayoutConstraint` with 7 parameters. Oh, and always better than [Visual Language Format](/2014/05/13/first-taste-with-visual-language-format/).

`NSLayoutConstraint.activate` is the magic method to activate all those constraints.

`NSLayoutConstraint.deactivate` is removing the constraints, and will be useful in animation.
