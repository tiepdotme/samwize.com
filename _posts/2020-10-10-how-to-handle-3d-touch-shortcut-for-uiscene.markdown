---
layout: post
title: "How to Handle 3D Touch Shortcut for UIScene"
date: 2020-10-10T13:32:25+08:00
categories: [UIScene, iOS]
---

Following up on the post on [deeplink for UIScene](/2020/10/09/how-to-handle-deeplink-for-uiscene/), this post will be on handling 3D/Force touch shortcuts for UIScene. Again, because of `UIScene`, the old way of using AppDelegate will no longer work. So this post is a refresh.

We also had covered [App Shortcuts](/2016/04/25/tutorial-on-creating-app-shortcut-with-3d-touch/), including setting up of dynamic shortcuts, so we won't cover here.


## 1. Setup static shortcuts

In Info.plist, static shortcuts can be configured like this:

```xml
<key>UIApplicationShortcutItems</key>
<array>
  <dict>
    <key>UIApplicationShortcutItemIconType</key>
    <string>UIApplicationShortcutIconTypeCaptureVideo</string>
    <key>UIApplicationShortcutItemTitle</key>
    <string>Record Video</string>
    <key>UIApplicationShortcutItemType</key>
    <string>recordVideo</string>
  </dict>
</array>
```

The `UIApplicationShortcutItemType` is like an identifier which will be used in code later.

For more information on setting up app shortcuts, refer to [Apple's doc](https://developer.apple.com/documentation/uikit/menus_and_shortcuts/add_home_screen_quick_actions).

## 2. Handle when app is running

Implement in `SceneDelegate` the protocol `UIWindowSceneDelegate`. Our custom method `handleShortcut()` will identify which shortcut it is and handle accordingly.

```swift
func windowScene(_ windowScene: UIWindowScene, performActionFor shortcutItem: UIApplicationShortcutItem, completionHandler: @escaping (Bool) -> Void) {
    completionHandler(handleShortcut(shortcutItem))
}

@discardableResult private func handleShortcut(_ shortcutItem: UIApplicationShortcutItem) -> Bool {
    switch shortcutItem.type  {
    case "recordVideo":
        // Handle this
    default: return false
    }
    return true
}
```

## 3. Handle cold start

As for cold start, you will need to handle in `scene(_:willConnectTo:options:)` instead.

```swift
func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
    // Your typical window setup, or even handleDeepLink..

    if let shortcutItem = connectionOptions.shortcutItem {
        handleShortcut(shortcutItem)
    }
}
```
