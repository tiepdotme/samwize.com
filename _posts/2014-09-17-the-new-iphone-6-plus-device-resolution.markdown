---
layout: post
title: "The new iPhone 6 Plus device resolution"
date: 2014-09-17 09:21:55 +0800
comments: true
categories: [iOS]
---

With the _bigger than bigger_ launch of iPhone 6 and iPhone 6 Plus, developers need to re-orientate again on the design landscape.

There is a [great illustration](http://www.paintcodeapp.com/news/iphone-6-screens-demystified) of the points system to rendered pixels to physical pixels by Paint Code. 

<!-- more -->

## Point System

The new iPhone 6 are indeed bigger in terms of point system, which is what developers use in coding the coordinates on the screen, therefore there is a big effect on app development.

- 3.5" uses 320x480

- 4" uses 320x568

- 4.7" uses 375×667

- 5.5" uses 414×736

Wow. It used be just 320x480. It used to be easy.


## Rendered pixels

Rendered pixels are based on the point system, with a scale factor of 1x, 2x or 3x.

The 3.5" and 4" iPhones use 1x or 2x. 

The 4.7" iPhone 6 will be 2x.

The 5.5" iPhone 6 Plus will be 3x.

The rendered pixels affects the graphics designers. There need to provide new Retina HD Display eg 3x.


## Actual Physical Pixel

All iPhones are pixel perfect, except iPhone 6 Plus.

Pixel perfect means each of the rendered pixel will be shown up as a physical pixel. 

But for iPhone 6 Plus, it is different. The rendered size is 1242×2208, while the physical size is 1080×1920.

It physically smaller, and Apple acheive that by downsampling the rendered to physical.

For development, an app should be designed as if it is 1242x2208 (the rendered pixel size).

The actual physical size can be ignored.


## What about iPad ?

Even. more. complex.

