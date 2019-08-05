---
layout: post
title: "Setup SceneDelegate Without Storyboard"
date: 2019-08-05T13:56:17+08:00
categories: [iOS]
---

New in iOS 13, multi-windows app requires [UIWindowSceneDelegate](https://developer.apple.com/documentation/uikit/uiwindowscenedelegate).

This post is an update to an [earlier post for pre-iOS 13](/2018/04/04/setup-appdelegate-without-storyboard/).

## 1. Remove `UISceneStoryboardFile`

Scenes are defined in your app `Info.plist`. There is a `UISceneStoryboardFile` key with default "Main" storyboard. Remove that key-value pair.

Your plist should look like this:

```xml
<key>UIApplicationSceneManifest</key>
<dict>
  <key>UIApplicationSupportsMultipleScenes</key>
  <false/>
  <key>UISceneConfigurations</key>
  <dict>
    <key>UIWindowSceneSessionRoleApplication</key>
    <array>
      <dict>
        <key>UILaunchStoryboardName</key>
        <string>LaunchScreen</string>
        <key>UISceneConfigurationName</key>
        <string>Default Configuration</string>
        <key>UISceneDelegateClassName</key>
        <string>$(PRODUCT_MODULE_NAME).SceneDelegate</string>
      </dict>
    </array>
  </dict>
</dict>
```

## 2. Remove `UIMainStoryboardFile`

Also in `Info.plist`, remove the storyboard key-value.

## 3. Setup `SceneDelegate`

Change the setup of the scene delegate to as such:

```swift
func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
    if let windowScene = scene as? UIWindowScene {
        let window = UIWindow(windowScene: windowScene)
        window.rootViewController = MyCustomViewController()
        self.window = window
        window.makeKeyAndVisible()
    }
}
```

The code creates your **custom view controller programmatically**, then setting it to the root of the window for the scene (phew)!
