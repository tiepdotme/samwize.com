---
layout: post
title: "Everything about custom fonts for iOS development"
date: 2014-03-12 11:15:52 +0800
comments: true
categories: iOS
---

## Basic Introduction

Starting for iOS 3.2, it is possible to use custom fonts in your iOS application. Read about [how to add custom fonts](http://samwize.com/2012/09/14/adding-and-using-custom-font-in-ios/) in my earlier post.

But there's much more to custom fonts.

<!-- more -->

## Set Global Appearance

You can also set the appearance in your AppDelegate `didFinishLaunchingWithOptions`.

     [[UILabel appearance] setFont:[UIFont fontWithName:@"CustomFont-Regular" size:17.0]];

However, this is a rather naive approach as it will fix the size (above exammple all font will be size 17).


## Set only font family

So a better approach is [this](http://stackoverflow.com/a/12139982/242682).

In each of your ViewController `viewDidLoad`, you can `setFontFamily` for the view and it's subview, while maintaining the font size. But you have to do it for every view controller.


## Use Runtime Attributes

You can change the font in Xcode using [user defined runtime attributes](http://www.davidahouse.com/blog/2013/02/10/bindings-for-ios-apps/).

There is a [good solution on Stackoverflow](http://stackoverflow.com/questions/9090745/custom-font-in-a-storyboard). Not sure who is the original author, but [cavaspod.io](http://canvaspod.io) uses the same code too. The technique is using category for UILabel, UIButton, UITextField, UITextView, and also UINavigationBar.

While good, it could be tedious to add key value for every UI element you want custom font for.


## IBCustomFonts Replacer

[IBCutomFonts](https://github.com/deni2s/IBCustomFonts) replace a natively supported font with a custom font. For example, replace all `HelveticaNeue-Regular` with `CustomFont-Regular`.

It uses method sizzling for `fontWithName` method, so it is a dangerous way.

Somehow, I could not get it to work correctly for iOS 7.

It's not recommended, although one of the cleanest way to replace all fonts.


## MoarFont for $10

This is [an app](http://pitaya.ch/moarfonts/) that shows up the custom font in Xcode. For $10. I didn't try it.


## Conclusion

I ended up using only 2 solutions:

1. Write code to [set font family](http://stackoverflow.com/a/12139982/242682) for a view which has many components

2. Use [cavaspod.io](http://canvaspod.io) to set runtime attributes for the rest

