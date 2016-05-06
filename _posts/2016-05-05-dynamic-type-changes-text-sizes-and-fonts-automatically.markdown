---
layout: post
title: "Dynamic Type to Adjust Text Sizes & Fonts Automatically"
date: 2016-05-05T13:55:03+08:00
categories: [iOS]
---

[iOS 7 introduces Dynamic Type](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/TransitionGuide/AppearanceCustomization.html#//apple_ref/doc/uid/TP40013174-CH15-SW4), a way for users to change their preferred text size from iPhone/iPad global Settings.

To know your user preferred settings:

```swift
UIApplication.sharedApplication().preferredContentSizeCategory
```

Currently, there are [7 different `UIContentSizeCategory` sizes](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIApplication_Class/#//apple_ref/doc/constant_group/Content_Size_Category_Constants), ranging from XS-XXXL.



## Text Styles

[Text Styles](https://developer.apple.com/library/ios/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/CustomTextProcessing/CustomTextProcessing.html#//apple_ref/doc/uid/TP40009542-CH4-SW65) are semantic descriptions.

There are a fixed number of descriptions, and currently in iOS 9, [10 styles are supported](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIFontDescriptor_Class/index.html#//apple_ref/doc/constant_group/Text_Styles):

- Title1, Title2, Title3
- Headline, Subheadline
- Body
- Footnote
- Caption1, Caption2
- Callout

This [table](http://stackoverflow.com/a/20510095/242682) shows the _actual font size_ when you combine your text style and your user preferred text size:

{%img http://i.stack.imgur.com/RiXd5.png %}


## Font Descriptor

Knowing what text style you want to use, you can then:

1. [Get the preferred UIFont](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIFont_Class/index.html#//apple_ref/occ/clm/UIFont/preferredFontForTextStyle:) `preferredFontForTextStyle:`
2. [Get the preferred UIFontDescriptor](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIFontDescriptor_Class/index.html#//apple_ref/occ/clm/UIFontDescriptor/preferredFontDescriptorWithTextStyle:) `preferredFontDescriptorWithTextStyle:`

We all (should) be familiar with `UIFont`.

What is this `UIFontDescriptor`?

`UIFontDescriptor` is introduced together with Dynamic Type in iOS 7, to provide a way to describe the attributes of a font, without actually creating a font object (which is more expensive).


## 1. System Font using UIFont

If you are okay with using the system font, then simply get the font with this 1 liner:

```swift
let titleFont = UIFont.preferredFontForTextStyle(UIFontTextStyleTitle1)
```


## 2. Custom Font using UIFontDescriptor

If you want a custom font, then you have to use the intermediary.

```swift
// Get the preferred descriptor instead
let titleDescriptor = UIFontDescriptor.preferredFontDescriptorWithTextStyle(UIFontTextStyleTitle1)

// Edit the trait and size, if needed
let boldTitleDescriptor = titleDescriptor.fontDescriptorWithSymbolicTraits(.TraitBold)

// Finally create the UIFont with the descriptor
let titleFont = UIFont(descriptor: boldTitleDescriptor, size: 0)
```

While you can edit the font to make it bold and change the font size etc.. there is a **pitfall**: You cannot change the font familiy or face.

To do that, you need to create `UIFontDescriptor` from scratch.

You will probably want to [scale your custom font](http://stackoverflow.com/a/20510095/242682) by a certain amount. Or use a [helper](http://useyourloaf.com/blog/scaling-dynamic-type-with-font-descriptors/) method.


## 3. Storyboard

If you are using storyboard, you are limited to the system font. 

Simply change Font > Text Style.

You will want to set the Minimum Font Scale (for UILabel). Do not use Minimum Font Size as that is deprecated.

As of Xcode 7.3, you cannot change the traits (eg. bold) from interface builder, but I belive the feature will come.



## Detecting Settings Changes

When a user change his preferred text size in Settings, you need to listen to `UIContentSizeCategoryDidChangeNotification`.

```swift
NSNotificationCenter.defaultCenter().addObserver(self, selector: #selector(ViewController.didChangePreferredContentSize), name: UIContentSizeCategoryDidChangeNotification, object: nil)
```

Then you need to update your views in your method `didChangePreferredContentSize`.

You might want to use [BNRDynamicTypeManager](https://github.com/bignerdranch/BNRDynamicTypeManager) to "watch" over the views that need to be updated.


## Testing on Simulator

Open Settings app, then go to **General > Accessibility > Larger text**.

However, note that simulator running on iOS 9.3 has a [bug](https://github.com/lionheart/openradar-mirror/issues/14563), so in some cases it might not work.

This is different from an actual device, which is under **Display & Brightness > Text Size**.


