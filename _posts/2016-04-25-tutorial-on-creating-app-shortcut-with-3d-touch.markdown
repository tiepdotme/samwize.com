---
layout: post
title: "Tutorial on Creating App Shortcut With 3D Touch"
date: 2016-04-25T17:02:59+08:00
categories: [iOS]
---

With 3D Touch (or Force Touch), Apple adds a [new way of interacting](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/Adopting3DTouchOniPhone/) with your app.

One of the new feature is having app shortcuts when you 3D touch the app icon.

{% img center https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/Adopting3DTouchOniPhone/Art/maps_directions_home_2x.png title:"Quick Actions as you 3D Touch" %}


## Type of shortcuts

1. Static - declared in `Info.plist`
2. Dynamic - created with [UIMutableApplicationShortcutItem](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIMutableApplicationShortcutItem_class/)

You can have up to 4 shortcuts. 


## Creating Static Shortcut Item

You add a [UIApplicationShortcutItems array](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW36) to `Info.plist`, like this:

{% img center /images/UIApplicationShortcutItems-in-Info-plist.png title:"Info.plist" %}

The title and subtitle strings are localized in `InfoPlist.strings`.


## Creating Dynamic Shortcut Item

You create 1 or more `UIMutableApplicationShortcutItem`, and set it to `UIApplication.sharedApplication().shortcutItems `. 

```swift
// Creating a shortcut
let shortcut1 = UIMutableApplicationShortcutItem(
      type: "com.myapp.shortcut1",
      localizedTitle: "Shortcut 1", 
      localizedSubtitle: nil,
      icon: UIApplicationShortcutIcon(templateImageName: "myimage"),
      userInfo: ["foo": "bar"])

// Set the shortcut items
UIApplication.sharedApplication().shortcutItems = [shortcut1, shortcut2]
```

Note: You can have up to 4 shortcuts, and the static shortcuts will be listed first.

In the example above, I used a custom icon image "myimage". You can easily use system provided icons such as `UIApplicationShortcutIconType.Search`.

You can pass more information via `userInfo`.


## Handling a Shortcut

Unsurprisingly, you handle in your `UIApplicationDelegate`.

```swift
// When app is already launched, performActionForShortcutItem will be called
func application(application: UIApplication, performActionForShortcutItem shortcutItem: UIApplicationShortcutItem, completionHandler: (Bool) -> Void) {
    completionHandler(handleShortcut(shortcutItem))
}

// When app is launched as a result of the shortcut, `didFinishLaunchingWithOptions` will be called
// If `launchOptions` has the shortcut item, then it need to return false 
// so that `performActionForShortcutItem` will not be called (which will result in calling `handleShortcut` twice)
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
    if let shortcutItem = launchOptions?[UIApplicationLaunchOptionsShortcutItemKey] as? UIApplicationShortcutItem {
        handleShortcut(shortcutItem)
        return false
    }
    
    return true
}

// Our private handler
private func handleShortcut(shortcutItem: UIApplicationShortcutItem) -> Bool {
    switch shortcutItem.type  {
    case "com.myapp.shortcut1":
        // Handle shortcut 1
    case "com.myapp.shortcut2":
        // Handle shortcut 2
    default: 
        return false
    }
    
    return true
}
```

From the `UIApplicationShortcutItem`, look for the `type` of shortcut. You can get more details from the `userInfo`.

That's it!


## Bonus: 3D Touch on Simulator

You can perform 3D Touch on simulator if you have a trackpad that supports Force Touch.

Go to **Hardware > Use Trackpad Force for 3D Touch**.

Tap lightly on the trackpad, and the quick actions shortcuts will be shown.


## Bonus: Peek and Pop

With 3D Touch, another feature you can implement is previewing a view controller.

[Krakendev](http://krakendev.io/peek-pop/) has a comprehensive tutorial on that.
