---
layout: post
title: "How to Change iOS App Icon Programmatically (iOS 10.3)"
date: 2017-08-20T16:09:07+08:00
categories: [iOS]
---

Changing the app icon is just [1 line of code](https://developer.apple.com/documentation/uikit/uiapplication/2806818-setalternateiconname) with `setAlternateIconName`, but there are many pitfalls, and the lack of guide on how to do it.

## The Code

Let's start with the easy part, which is the code to change the app icon programmatically.

```swift
if #available(iOS 10.3, *) {
    let newAppIconName = "AppIcon-2"

    guard UIApplication.shared.alternateIconName != newAppIconName else { return }

    UIApplication.shared.setAlternateIconName(newAppIconName)
}
```

You might thought you can put the code in app delegate when the app is launched, but no, that will have this un-helpful error:

> Error Domain=NSCocoaErrorDomain Code=3072 "The operation was cancelled."

## Pitfall: Code must be called in view controller

You must call `setAlternateIconName` only in view controller, and in main thread.

Why?

Because the user will be shown this alert:

![](/images/appicon-change-alert.jpg)

_If you want to run the code in app delegate, make sure it is run in main queue, and visible in the main window._

## Pitfall: You cannot avoid the system alert

The alert and the text "You have changed the icon for ..." cannot be changed, nor removed.

In [Apple Human Interface Guildeline (HIG)](https://developer.apple.com/ios/human-interface-guidelines/graphics/app-icon/), they did mention that changing the app icon is a user triggered action.

_You can perform tricks to dismiss the alert, or cover with another screen, but that is against the HIG._

This is also why in the code there is a `guard` statement. It prevents setting the icon name again, and therefore triggering the alert, if it already is that icon.

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

You may wonder what why "AppIcon-2" is repeated in `CFBundleIconFiles`?

That is actually the image file name (less the "png" or "@2x"). It need not be necessary the same as the key in `CFBundleAlternateIcons`. For simplicity we used the same name.

**Important**: If you need to support for iPad, read the last section on a critical pitfall.

## Pitfall: Cannot use asset catalog

Strangely, alternative icons CANNOT be added to your asset catalog, unlike the primary icon.

If you do that, you will see an un-helpful error again.

You have to add "AppIcon-2.png", "AppIcon-2@2x.png", "AppIcon-2@3x.png" (180x180) to your app target.

## Pitfall: You need `completionHandler`

Although `setAlternateIconName` can accept a nil `completionHandler`, as declared in its signature, you shouldn't because the app with CRASH, if somehow there is error.

As [dlbuckley](https://forums.developer.apple.com/thread/71463) pointed out (as of Aug 2017),

> This API isn't quite ready to be released to us thugs

## Pitfall: Not working or Crash in iPad

This is another common one, and critical, because for the neglected iPad.

You could run into error like this:

```
Error Domain=NSCocoaErrorDomain Code=4 "The file doesn’t exist."
UserInfo={NSUnderlyingError=0x60000005e0c0 {Error Domain=LSApplicationWorkspaceErrorDomain
Code=-105 "iconName not found in CFBundleAlternateIcons entry"
UserInfo={NSLocalizedDescription=iconName not found in CFBundleAlternateIcons entry}}}
```

In Apple's [obscure documentation](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-SW14), it pointed out:

> Important: If your app contains iPad-specific versions of its icons, the system does not fall back to the alternate icons declared in the platform-agnostic version of CFBundleIcons key. Therefore, if you include any alternate icons in the CFBundleIcons key, you must include them again in your CFBundleIcons~ipad variant.

That means, in your "Info.plist", you need to have a separate entry with `CFBundleIcons~ipad` key!

So, copy whatever in `CFBundleIcons` and add for `CFBundleIcons~ipad`.
