---
layout: post
title: "iOS Guide to Multiple Device Resolutions"
date: 2012-10-29 23:23
comments: true
categories: iOS
---

With every iOS updates and new release of devices from Apple, there are more things developers have to do.

In order to support these new devices, you first need to understand these devices.

<!-- more -->

## Device Resolutions ##

Apple provides a [table matrix](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html#//apple_ref/doc/uid/TP40006556-CH14-SW1) on the App Icons, Screen size, and tool bar icon sizes for the various devices.

This table summarizes the resolutions:

|                | iPhone 5          | iPhone (Retina)   | iPhone        | iPad (Retina) | iPad          |
| -------------- | ----------------- | ----------------- | ------------- | ------------- | ------------- |
| Actual         | 640 x 1136        | 640 x 960         | 320 x 480     | 1536 x 2048   | 768 x 1024    
| Default.png    | 640 x 1136        | 640 x 960         | 320 x 480     | 1536 x 2008   | 768 x 1004    
| App Icon       | 114 x 114         | 114 x 114         | 57 x 57       | 144 x 144     | 72 x 72       
| Toolbar icon   | 40 x 40           | 40 x 40           | 20 x 20       | 40 x 40       | 20 x 20       

Note that for iPads, the Default.png should not include the status bar (20px).

## Image Resource File ##
Apple introduced an easy way to load [images](http://developer.apple.com/library/ios/#documentation/2DDrawing/Conceptual/DrawingPrintingiOS/SupportingHiResScreensInViews/SupportingHiResScreensInViews.html#//apple_ref/doc/uid/TP40010156-CH15) automatically for the devices. 

The rule goes like this:

- Standard: `<ImageName><device_modifier>.<filename_extension>`

- High resolution: `<ImageName>@2x<device_modifier>.<filename_extension>`

For [example](http://developer.apple.com/library/ios/#documentation/Cocoa/Conceptual/LoadingResources/ImageSoundResources/ImageSoundResources.html#//apple_ref/doc/uid/10000051i-CH7-SW1),

- MyImage.png - Default version of an image resource.
- MyImage@2x.png - High-resolution version of an image resource for devices with Retina displays.
- MyImage~iphone.png - Version of an image for iPhone and iPod touch.
- MyImage@2x~iphone.png - High-resolution version of an image for iPhone and iPod touch devices with Retina displays.
- MyImage~ipad.png - Version of an image for iPad
- MyImage@2x~ipad.png - High-resolution version of an image for iPad with Retina displays.

With that knowledge, you could load a UIImage 

    UIImage *anImage = [UIImage imageNamed:@"MyImage"];

and the appropriate resource file will be loaded.


## iPhone 5 - taller screen ##

Unfortunately, for the taller iPhone 5, it does not automatically load `MyImage@2x~iphone5.png`.

You could use a [handy](http://stackoverflow.com/questions/5088945/use-2x-retina-images-for-ipad-in-universal-app-and-does-apple-prefer-native-ap) UIImage category to load.




