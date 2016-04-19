---
layout: post
title: "Pitfall: Core Graphics Drawing Blurry Lines"
date: 2016-04-19T16:47:18+08:00
categories: [iOS]
---

There is a common pitfall with using Core Graphics API to draw.

It is to do with the retina devices, which has more pixels packed in 1 point.

Take this example of drawing a round rect:

```swift
// Draw a 1 pt border line with radius 5 in the rect
let rect = CGRect(x: 0, y: 0, width: 200, height: 100)
let borderWidth: CGFloat = 1
let radius: CGFloat = 5

UIGraphicsBeginImageContext(rect.size)
let context = UIGraphicsGetCurrentContext()

// Note: Because stroke will overflow, we need to make the actual round rect smaller
// http://stackoverflow.com/a/11176658/242682
let roundRect = CGRectMake(borderWidth/2, borderWidth/2, (rect.width - borderWidth), (rect.height - borderWidth))

// Fill and stroke
let path = UIBezierPath(roundedRect: roundRect, cornerRadius: radius)
UIColor.whiteColor().setFill()
path.fill()
UIColor.redColor().setStroke()
path.lineWidth = borderWidth
path.stroke()

// Draw it
CGContextAddPath(context, path.CGPath);

// Get the final image from context
let img = UIGraphicsGetImageFromCurrentImageContext()
UIGraphicsEndImageContext()
```

When the image is shown on a retina device (@2x or @3x), the 1pt line border will be blurry.

The [solution](http://stackoverflow.com/a/6965894/242682) is simple:

```swift
UIGraphicsBeginImageContextWithOptions(rect.size, false, 0.0)
```

Use [`UIGraphicsBeginImageContextWithOptions` API](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIKitFunctionReference/#//apple_ref/c/func/UIGraphicsBeginImageContextWithOptions
) instead of the old `UIGraphicsBeginImageContext`. 

The third parameter passed in is the **scale**. Passing in `0.0` will use the device scale automatically.

Remember: Drawing is always specified in **points**, but when you "begin image context", you have to use `UIGraphicsBeginImageContextWithOptions` to specific the correct scale.
