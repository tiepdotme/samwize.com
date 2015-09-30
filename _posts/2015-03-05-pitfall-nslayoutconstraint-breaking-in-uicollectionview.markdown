---
layout: post
title: "Pitfall: NSLayoutConstraint breaking in UICollectionView"
date: 2015-03-05 00:28:31 +0800
comments: true
categories: [iOS]
---

I encountered a weird error when using `UICollectionView`, where the storyboard is using autolayout.

This is the error:

```
Unable to simultaneously satisfy constraints.
    Probably at least one of the constraints in the following list is one you don't want. Try this: (1) look at each constraint and try to figure out which you don't expect; (2) find the code that added the unwanted constraint or constraints and fix it. (Note: If you're seeing NSAutoresizingMaskLayoutConstraints that you don't understand, refer to the documentation for the UIView property translatesAutoresizingMaskIntoConstraints) 
(
    "<NSLayoutConstraint:0x7fda6411de70 H:[UIButton:0x7fda6411dc90(13)]>",
    ...
    "<NSAutoresizingMaskLayoutConstraint:0x7fda61cf61e0 h=--& v=--& H:[UIView:0x7fda6411d500(50)]>"
)

Will attempt to recover by breaking constraint 
<NSLayoutConstraint:0x7fda6411de70 H:[UIButton:0x7fda6411dc90(13)]>
```

<!-- more -->

It doesn't crash as it is able to recover by breaking constraint.

## It is weird becase:

- If I try to solve by breaking the constraint, it will not layout as I wanted

- I checked every constraints and they are valid and minimal; I need not remove any unwanted constraints


## Solution

The problem lies with the view cell.

Somehow, the `contentView` must set `translatesAutoresizingMaskIntoConstraints` to `NO`.

```objc
    [cell.contentView setTranslatesAutoresizingMaskIntoConstraints:NO];
```

This will similarly apply to `UITableViewCell`.

It seems like the content view cell must use auto resizing mask.
