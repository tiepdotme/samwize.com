---
layout: post
title: "Adding and Using Custom Font in iOS"
date: 2012-09-14 00:13
comments: true
categories: iOS
---

Custom font can be easily added and used in iOS 3.2 or above. Yet it is a very popular [question](http://stackoverflow.com/questions/360751/can-i-embed-a-custom-font-in-an-iphone-application) on for iOS Developers.

## The Steps

1. Add the otf/ttf font into your Resources folder. The name of my font is **BentonSansComp-Book.otf**

2. Edit **info.plist** and add a key call `UIAppFonts` - value type should be `Array`. The friendly name for `UIAppFonts` is `Fonts provided by application`.

3. In `item0` of the array enter the name of the font you added -- in my case, **BentonSansComp-Book.otf**

4. Find the font name.

	IMPORTANT: The font name is not necessary the filename. Open with **Font Book.app** > **Show font info** > look for **PostScript name**. That's the font name you should use. For my font, it happens that the filename == font name.

5. Unfortunately, Xcode interface builder does not let you change to your custom font. So you got to code it!

```objc
[myLabelView setFont:[UIFont fontWithName:@"BentonSansComp-Book" size:16]];
```

## Framework

When you have the font in a framework (not application), then you need to handle differently because of [bundle](/2018/04/27/what-is-bundle/).

```swift
let bundle = Bundle(for: SomeClass.self) // SomeClass is in the framework along with the font resource
registerFont("BentonSansComp-Book", extension: "otf", in: bundle)

func registerFont(_ fontName: String, extension: String, in bundle: Bundle) {
    guard let fontURL = bundle.url(forResource: fontName, withExtension: `extension`) else { return }
    CTFontManagerRegisterFontsForURL(fontURL as CFURL, CTFontManagerScope.process, nil)
}
```
