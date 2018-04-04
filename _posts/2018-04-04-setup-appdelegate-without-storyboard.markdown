---
layout: post
title: "Setup AppDelegate Without Storyboard"
date: 2018-04-04T22:29:45+08:00
categories: [iOS]
---

When you create a new project in Xcode, the default boilerplate includes a `Main.storyboard` with a view controller that will be initialized when the app is launched.

If you are going with no-storyboard approach, then you need 2 steps to remove the storyboard.

## 1. Remove Main.storyboard

Delete the storyboard file.

The only reference to the storyboard file is in the target settings.

Go to App Target > General > Deployment Info > Main Interface > delete "Main".

![](/images/xcode-main-interface.jpg){:.border}

Leave it blank, as we will create the main interface with code in the next step.

## 2. Create main window

We need to create the window manually, when the app is launched.

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
    window = UIWindow(frame: UIScreen.main.bounds)
    let v = ViewController()
    window!.rootViewController = v
    window!.makeKeyAndVisible()
    return true
}
```

That's it! Not that hard at all without storyboard.
