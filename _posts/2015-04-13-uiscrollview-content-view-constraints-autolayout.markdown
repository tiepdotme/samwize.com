---
layout: post
title: "UIScrollView Content View Contraints"
date: 2015-04-13 21:15:35 +0800
comments: true
categories: [iOS]
---

The `Content` view in a `UIScrollView` is very unique when it comes to configuring a scroll view with auto layout.

In short, the constraints needed:

1. You must have only 1 child view in `UIScrollView`, which we refered to as `Content` view

2. `Content` is usually pinned up, down, left and right to the `UIScrollView` 

3. In addition, `Content` must define it's width and height, which is the scrollable area. In most case you will have the width equal to the `UIScrollView`. The height is set to 888 in example below, but usually you will have the views within in to determine the overall height.

{%img center /images/UIScrollView-content-setup.png %}



