---
layout: post
title: "How to Change iOS App Icon Programmatically (iOS 10.3)"
date: 2017-08-17T16:09:07+08:00
categories: []
---

Changing the app icon is just [1 line of code](https://developer.apple.com/documentation/uikit/uiapplication/2806818-setalternateiconname) with `setAlternateIconName`, but there is quite some pitfalls, and the lack of guide on how to do it.

## The Code

Let's start with the easy part, which is the code to change the app icon programmatically.

```swift
if #available(iOS 10.3, *) {
    UIApplication.shared.setAlternateIconName("AppIcon-2") { error in
        if let error = error {
            print(error.localizedDescription)
        } else {
            print("Success!")
        }
    }
}
```

You might thought you can put the code in app delegate when the app is launched, but no, that will have this un-helpful error:

> Error Domain=NSCocoaErrorDomain Code=3072 "The operation was cancelled."

## Pitfall: Code must be called in view controller

You must call `setAlternateIconName` only in view controller.

Why?

Because the user will be shown this alert:

[screenshot]

_If you want to run the code in app delegate, make sure it is run in main queue._

## Pitfall: You cannot avoid the system alert

The alert and the text ("You have changed the icon for ...") cannot be changed, nor removed. 

In [Apple Human Interface Guildeline (HIG)](https://developer.apple.com/ios/human-interface-guidelines/graphics/app-icon/), they did mention that changing the app icon should be a user triggered action.

_You can perform tricks to dismiss the alert, or cover with another screen, but that is against the HIG._

## The Info.plist

Now that you understand the code and how it works, let's setup the app's Info.plist.

The following has to be added:

```xml
<key>CFBundleIcons</key>
<dict>
  <key>CFBundlePrimaryIcon</key>
  <dict>
    <key>CFBundleIconFiles</key>
    <array>
      <string>AppIcon</string>
    </array>
    <key>UIPrerenderedIcon</key>
    <true/>
  </dict>
  <key>CFBundleAlternateIcons</key>
  <dict>
    <key>AppIcon-2</key>
    <dict>
      <key>CFBundleIconFiles</key>
      <array>
        <string>AppIcon-2</string>
      </array>
      <key>UIPrerenderedIcon</key>
      <true/>
    </dict>
  </dict>
</dict>
```

The [keys are explained](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-SW13), but in short:

- `CFBundleIcons`
  - `CFBundlePrimaryIcon`
  - `CFBundleAlternateIcons` - dictionary of alternative icons, with the key name as the name that you will use in code eg. "AppIcon-2"

You may wonder what the repeated "AppIcon-2" in `CFBundleIconFiles`?

That is actually the image file name (less the "png" or "@2x"). It need not be necessary the same as the key in `CFBundleAlternateIcons`. For simplicity we used the same name.

## Pitfall: Cannot use asset catalog

You might thought the alternative icons can be added to your asset catalog, like the primary icon does.

But no no, if you did that, you will see an un-helpful error again.

You have to add "AppIcon-2.png", "AppIcon-2@2x.png", "AppIcon-2@3x.png" to your app target.

## Pitfall: 