---
layout: post
title: "The Thing About Top & Bottom Autolayout Guide"
date: 2016-09-23T15:32:08+08:00
categories: [iOS]
---

iOS 7 introduced the concept of a translucent tab/nav/status bar.

In doing so, view controllers "extend" their views so that it is beneath the bar, giving a _there-but-not-there-blurry_ effect. 

To help view controllers to adapt to this iOS 7 concepts, know this:

- the view is now expanded to beneath the bar(s)
- to know the area that is NOT beneath the bar, you have to use the top & bottom layout guide (these are 2 properties in view controller)
- hence when you add subviews, they should usually be **between** the layout guides
- however, for scroll view, they should **extend** beyond the guides, with an inset using the layout guides


## If you have Scroll View with Translucent Bar

Assuming you use scroll view (including table/collection views) with a translucent nav and tab bar, this is how you configure your storyboard for the view controller:

- Enable **Adjust Scroll View Insets**
- Enable **Under Top Bars**
- Enable **Under Bottom Bars**
- Pin the scroll view to **Superview** top, bottom, left, right

Why this works?

When you enable **Under Top/Bottom Bars**, it tells the view controller's root view to extend the edges. Programatically you can set [`edgesForExtendedLayout`](https://developer.apple.com/reference/uikit/uiviewcontroller/1621515-edgesforextendedlayout).

We make the scroll view the same rect as it's superview, that is the view controller's root view.

Then enable **Adjust Scroll View Insets** so that view controller automatically make the inset fall between the top & bottom layout guide!


## Trivial

The top and bottom layout guides are [`UILayoutSupport`](https://developer.apple.com/reference/uikit/uilayoutsupport) protocol implemented by `UIViewController`.

It is not `UILayoutGuide`. 

[`UILayoutGuide`](https://developer.apple.com/reference/uikit/uilayoutguide) is a different object. It is a useful object if you have constructed _dummy views_, or container views, merely to help with autolayout. 

It replace the heavyweight dummy views with this lightweight object, which you can continue to place autolayout constraints with other views.

