---
layout: post
title: "Updating App for iPhone 5"
date: 2012-10-22 01:14
comments: true
categories: iOS
published: true
---

This post is a guide to updating existing app for iPhone 5.

As you already knew, the new iPhone 5 introduced a taller screen at 4 inch, while all iPhones before is at 3.5 inch. 

There are a couple of steps that you have to do to support the taller screen, and it really depends on how you develop your app in the first place. You might use all of the techniques below, or none. 

These techniques are inspired by [answers](http://stackoverflow.com/questions/12395200/how-to-develop-or-migrate-apps-for-iphone-5-screen-resolution) [on](http://stackoverflow.com/questions/12518879/extend-app-for-iphone-5-best-practice) [Stackoverflow](http://stackoverflow.com/questions/12519110/what-to-name-images-for-iphone-5-screen-size). The result is a `Device` class I created at [https://gist.github.com/3926123](https://gist.github.com/3926123)

<!-- more -->

## 1. Default-568h@2x.png ##

There isn't a setting to specify a project supports iPhone 5. Instead, a presence of a `Default-568h@2x.png` signify that it supports iPhone 5.

The size is 640 x 1136.

Xcode 4.5 will actually offer to add one automatically.


## 2. Set to Full Screen at launch for MainWindow.xib ##

If your tab bar didn't work properly (such as don't respond to click) after you added `Default-568h@2x.png`, then it is likely due to the nib configuration. 

You have to go to interface builder, select the window object, and [enable](http://stackoverflow.com/a/12699347/242682) `Full Screen at launch`. 


## 3. Code to detect 4-inch screen ##

You can detect the 4-inch screen size with the following method:

```objc
/** Returns true if it is a iPhone/iPod with 4 inch tall screen */
+(BOOL)isScreen4Inch {
    if(UI_USER_INTERFACE_IDIOM() == UIUserInterfaceIdiomPhone) {
        CGRect screenBounds = [[UIScreen mainScreen] bounds];
        if (screenBounds.size.height == 568)
            return YES;
    }
    return NO;
}
```

I have this in `Device.h`, so import and use.

```objc
if ([Device isScreen4Inch]) {
	// Code for 4 inch
} else {
	// Code for anything else (3.5 inch or iPad)
}
```


## 4. Use Auto Layout for iOS6 ##

If you are supporting only iOS6 and above, you should use Auto Layout. 

You can read up a tutorial by [Ray Wenderlich](http://www.raywenderlich.com/20881/beginning-auto-layout-part-1-of-2).



## 5. Set auto resizing mask for pre iOS6 ##

If you are still supporting pre iOS6, you still need auto resizing mask for your views to stretch and re-position automatically.

Refer to [Apple UIView doc](http://developer.apple.com/library/ios/#documentation/uikit/reference/uiview_class/uiview/uiview.html) on `UIViewAutoresizing`.

This 'feature' was in iOS ever since iOS is available. If you design your apps mostly using Xcode Interface Builder, this would one of the most needed change. 

You could change a subview to auto resize its width/height, and fix it's margin to left/right/top/bottom. 


## 6. Stretch Images ##

If you are using Xcode Interface Builder, change the UIImageView `mode` to `Scale to Fill` (or others).

Then change the `Stretching` properties. The values are a fraction of the image size. For example, X and Y are 0.5, and Width and Height are 0.1, it means stretch the portion of the image at center with 10% width and height.

Your images can also be [stretched](http://mobiledevelopertips.com/user-interface/ios-5-uiimage-and-resizableimagewithcapinsets.html) with UIImage `resizableImageWithCapInsets:`.

But that works only if your image has portion that can be stretched without distortion. If it doesn't, you have to create a new image `-568@2x` image, and use the trick covered next to load it automatically.


## 7. Load -568h@2x images ##

Unlike `@2x` which are loaded automatically for Retina Display, the `-568h@2x` are not loaded automatically for the taller iPhone (except the `Default-568h@2x.png`).

I use a convenient method below:

```objc
+(UIImage*)imageNamedFor568h:(NSString*)imageName {
    if ([Device isScreen4Inch]) {
        NSString *imageNameFor568h = [NSString stringWithFormat:@"%@-568h.%@", [imageName stringByDeletingPathExtension], [imageName pathExtension]];
        return [UIImage imageNamed:imageNameFor568h];
    }
    return [UIImage imageNamed:imageName];
}
```

Using category, I introduced the method for UIImage.

To use, I would have to change the methods `[UIImage imageName:@"foo.png"]` to `[UIImage imageNameFor568h:@"foo.png"]`.


## 8. iOS Simulator ##

To run and test on a 4 inch iPhone, select **Hardware** > **Device** > **iPhone (Retina 4-inch)**.



## 9. Last words.. ##

It largely depends on how you code your existing apps. 

Use the above techniques as needed, and handily use the [Device class](https://gist.github.com/3926123) methods.

If you use any third party libraries, you would also need to update their libs with support for `armv7s` (a new architecture for iPhone5). Popular libs are already updated at this point of writing eg. Flurry, Google Admob, and other ad networks.



