---
layout: post
title: "How to use UIScrollView with Autolayout"
date: 2014-03-14 11:04:40 +0800
comments: true
categories: iOS
---

_UPDATE: Refer to this [simple post](/2015/04/13/uiscrollview-content-view-constraints-autolayout/) on how to setup the constraints for the content view_

[Atomic Spin](http://spin.atomicobject.com/2014/03/05/uiscrollview-autolayout-ios/?utm_source=maniacdev-ao&utm_medium=social&utm_campaign=uiscrollview-autolayout-ios) has a detailed post explaining how to use UIScrollView with [Auto Layout](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40010853-CH13-SW1).

Here are my own essential steps with explanations.

<!-- more -->

### 1. Setup The Components

Setup the storyboard: **ViewController View (white)** >> **Scroll View (green)** >> **Content View (grey)** >> **UITextField**

{%img center /images/uiscrollview-setup.png %}

Problem: When you rotate to landscape, the **Scroll View** will not fill up the width.

{%img center /images/uiscrollview-problem1.png %}


### 2. Fix the Scroll View

This is easy autolayout problem.

Pin Top/Bottom/Left/Right of **Scroll View** to the **ViewController View**. The 4 constraints will make Scroll View width/height adjust accordingly when in landscape.

{%img center /images/uiscrollview-pin.png %}

Next Problem: When you rotate to landscape, the **Content View** will not fill up the width.

{%img center /images/uiscrollview-problem2.png %}


### 3. Fix the Content View

Here's the tricky part where you could fall into 2 pitfalls.

Pitfall 1: You might think you can pin Top/Bottom/Left/Right of **Content View** to **Scroll View**. However, UIScrollView is "special" here. The constraints don't affect Content View, but rather `scrollView.contentSize` (different thing!). See [pic](http://d37rcl8t6g8sj5.cloudfront.net/wp-content/uploads/uiscrollview-with-constraints-on-content.png) from Atomic object.

Pitfall 2: You might think you can pin Top/Bottom/Left/Right of **Content View** to **ViewController View** (it's grandparent container). However, unfortuantely, autolayout can't do that in storyboard.

Your attempt in Pitfall 2 is close to the solution. What you have to do is write the code in `viewDidLoad`.

```objc
    // Remember to setup the IBOutlet for contentView in .h file
    @property (weak, nonatomic) IBOutlet UIView *contentView;

    // The constraints that can only work with code
    - (void)viewDidLoad
    {
        [super viewDidLoad];

        NSLayoutConstraint *leftConstraint = [NSLayoutConstraint constraintWithItem:self.contentView
                                                                          attribute:NSLayoutAttributeLeading
                                                                          relatedBy:0
                                                                             toItem:self.view
                                                                          attribute:NSLayoutAttributeLeft
                                                                         multiplier:1.0
                                                                           constant:60];
        [self.view addConstraint:leftConstraint];

        NSLayoutConstraint *rightConstraint = [NSLayoutConstraint constraintWithItem:self.contentView
                                                                           attribute:NSLayoutAttributeTrailing
                                                                           relatedBy:0
                                                                              toItem:self.view
                                                                           attribute:NSLayoutAttributeRight
                                                                          multiplier:1.0
                                                                            constant:-60];
        [self.view addConstraint:rightConstraint];
    }
```

In the code above, I create a left 60 and right 60 space from `contentView` to `self.view` (it's grandparent). Now it's better.

{%img center /images/uiscrollview-code-constraints.png %}

Next Probem: Wait. It doesn't scroll?!


### 4. Fix Scrolling

If you have followed through so far, there will be missing constraints for the Scroll View. Without adding these missing constraints, Scroll View will not be able to scroll.

Pin Top/Bottom/Left/Right of **Content View** to **Scroll View** - this aforementioned "special" constraint is to tell **Scroll View** the `contentSize`, aka the scrollable area. As quoted in [Apple Technical Note TN2154](https://developer.apple.com/library/ios/technotes/tn2154/_index.html):

>  To make this work with Auto Layout, the top, left, bottom, and right edges within a scroll view now mean the edges of its content view.

In the above quoted, "content view" refers to **Scroll View** `contentSize`, not the **Content View** used in our scenario. Same name, different thing.

Now that **Scroll View** knows the scrollable area, it can then scroll.

Next Problem: Keyboard obscure the views


### 5. Fix the keyboard

To accomodate for the keyboard, the proper way is to adjust the `contentInset`. Figure 1-3 of [Apple documentation](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/UIScrollView_pg/CreatingBasicScrollViews/CreatingBasicScrollViews.html) will explain better what is this `contentInset` (as well as `contentSize`).

Another Pitfall: The keyboard notification will give a wrong size. Solution is to [convert the keyboard frame correctly](http://stackoverflow.com/a/14351009/242682).

Solving view obscured by keyboard is a very common issue. Not gonna repeat here, so simply follow the last section of the guide from [Atomic Spin](http://spin.atomicobject.com/2014/03/05/uiscrollview-autolayout-ios/).
