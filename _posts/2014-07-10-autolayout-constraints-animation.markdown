---
layout: post
title: "Animating Autolayout Constraints"
date: 2014-07-10 10:43:59 +0800
comments: true
categories: iOS
---

As described in [Apple Autolayout Guide](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/AutolayoutPG/AutoLayoutbyExample/AutoLayoutbyExample.html), your `UIView` animation block should look like this:

<!-- more -->

```objc
[containerView layoutIfNeeded]; // Ensures that all pending layout operations have been completed
[UIView animateWithDuration:1.0 animations:^{
  // Make all constraint changes here
  // For example:
  someViewConstraint.constant = 80;
  [containerView layoutIfNeeded]; // Forces the layout of the subtree animation block and then captures all of the frame changes
}];
```