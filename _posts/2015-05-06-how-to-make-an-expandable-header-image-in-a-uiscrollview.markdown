---
layout: post
title: "How to make an expandable header image in a UIScrollView"
date: 2015-05-06 11:07:27 +0800
comments: true
categories: [iOS]
---

A common UI design pattern is to make an image fill the header as you scroll beyond the top, like this:

{%img center /images/expandable-header-uiscrollview.gif %}

This post will show you how to do that, easily.

<!-- more -->

Implement the `UIScrollViewDelegate` method as follows:

```objc
- (void)scrollViewDidScroll:(UIScrollView *)scrollView {
    if (scrollView.contentOffset.y <= 0) {
        CGAffineTransform translate = CGAffineTransformMakeTranslation(0, scrollView.contentOffset.y/2.0);
        CGFloat orgHeight = _photoHeightConstraint.constant;
        CGFloat scaleFactor = (orgHeight - scrollView.contentOffset.y) / orgHeight;
        CGAffineTransform translateAndZoom = CGAffineTransformScale(translate, scaleFactor, scaleFactor);
        _photoImageView.transform = translateAndZoom;
    }
}
```

In the code above `_photoImageView` is the image view that we want to make expandable, and `_photoHeightConstraint` is a NSLayoutConstraint for the height of `_photoImageView`.

That's all for the code!

When the scroll view `contentOffset` changes, perform a zoom and translate transformation.
