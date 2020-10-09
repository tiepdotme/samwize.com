---
layout: post
title: "How to Handle Deeplink for UIScene"
date: 2020-10-09T16:35:31+08:00
categories: [Scene, deeplink]
---

`UIScene` was introduced in iOS 13, and it replaced many functionalities of `AppDelegate`.

If you use `UIScene`, then the old way of using `application(_:open:options:)` in `AppDelegate` will NOT work anymore.

## 1. Basic setup

To handle deeplink, the Info.plist must have the `CFBundleURLTypes` key added. In the example, we will have our app support a `myschema://` deeplink.

```xml
<key>CFBundleURLTypes</key>
<array>
	<dict>
		<key>CFBundleTypeRole</key>
		<string>Editor</string>
		<key>CFBundleURLSchemes</key>
		<array>
			<string>myschema</string>
		</array>
	</dict>
</array>
```

## 2. Handle when app is running

In `SceneDelegate`, add the following method:

```swift
func scene(_ scene: UIScene, openURLContexts URLContexts: Set<UIOpenURLContext>) {
    for context in URLContexts {
        // Your method to handle the deeplink url
        handleDeepLink(context.url)
    }
}
```

Note: There can be multiple URLs. The delegate method provides a set of `UIOpenURLContext`. We are only interested in only the `url`. There is also an `options` in the context.

## 3. Handle cold start

When the app is not yet running, you need to handle in `scene(_:willConnectTo:options:)` instead.

```swift
func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
    // The typical window set up
    if let windowScene = scene as? UIWindowScene {
        let window = UIWindow(windowScene: windowScene)
        window.rootViewController = MainViewController()
        self.window = window
        window.makeKeyAndVisible()
    }

    for context in connectionOptions.urlContexts {
        handleDeepLink(context.url)
    }
}
```

The code is similar, and again we use the same `handleDeepLink()`.
