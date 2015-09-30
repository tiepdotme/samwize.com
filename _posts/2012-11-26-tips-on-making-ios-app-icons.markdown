---
layout: post
title: "Tips on making iOS App Icons"
date: 2012-11-26 23:42
comments: true
categories: [iOS, Graphics]
---

Making a beautiful iOS app icon takes some hard work. Even more work now that there are more devices with different resolutions.

This post is to provide some of the tips in the making Apple's trademarked iPhone rounded icon.

<!-- more -->

## Understanding the resolutions ##

There is quite a number of different resolutions that is needed for 1 app. 

Refer to this [Apple document](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html#//apple_ref/doc/uid/TP40006556-CH14-SW1).

Note that every icon can be different in terms of how it looks. So in theory you can have a 57x57 that is totally different looking from your 512x512.



## Icon Template ##

There are many template around. The most popular (now) is from [http://appicontemplate.com/](http://appicontemplate.com/).

Using Photoshop smart objects, the template let you edit the biggest size, and it will automatically scale down to the smaller sizes, and export them automatically.



## Rounded corner radius ##

If you are not using the template or need a specific resolution, it is important that you know the rounded corner radius for each of the icon reolution. This [discussion](http://stackoverflow.com/a/10239376/242682) explained well.

To summarize, 

> Rounded corner radius = icon length x 10/57

That is,

- Icon512.png - 512px - 89.825
- Icon.png - 57px - 10
- Icon@2x.png - 114px - 20
- Icon-72.png - 72px - 12.632
- Icon-72@2x.png - 144px - 25.263
- Icon-Small.png - 29px - 5.088
- Icon-Small@2x.png - 58px - 10.175


## Mask ##

You can get a set of mask from within Xcode. 

Open the following directory.

	open /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator6.0.sdk/System/Library/PrivateFrameworks/MobileIcons.framework

But there is also a [way](http://stackoverflow.com/a/1834558/242682) to programmatically set a round corner masking:

```objc
	imageView.layer.cornerRadius = 10.0;
	imageView.layer.masksToBounds = YES;
	imageView.layer.borderColor = [UIColor lightGrayColor].CGColor;
	imageView.layer.borderWidth = 1.0;
```