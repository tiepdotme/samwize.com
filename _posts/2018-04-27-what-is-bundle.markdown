---
layout: post
title: "What is Bundle?"
date: 2018-04-27T17:01:16+08:00
categories: [iOS]
---

[Bundle](https://developer.apple.com/documentation/foundation/bundle) represents a directory. It [groups resources](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFBundles/AboutBundles/AboutBundles.html) together.

## Application

In a typical application, all of your assets, images, strings, resources and code will be in the "main bundle".

To be convenient, resource classes provide sensible default to use.

```swift
// NSLocalizedString function signature has a default bundle
NSLocalizedString(_ key: String, tableName: String? = default, bundle: Bundle = default, value: String = default, comment: String) -> String
```

The [`default`](https://stackoverflow.com/q/24991791/242682) is `Bundle.main`. So you don't have to specify explicitly.

Without surprise, [`UIImage` initialization](https://developer.apple.com/documentation/uikit/uiimage/1624146-init) uses the main bundle too. And if you want to specific another bundle you could use the [other init](https://developer.apple.com/documentation/uikit/uiimage/1624154-init).

But the default to use main bundle is not applicable to frameworks and playgrounds.

## Framework

When you are developing a framework, you should NOT use `Bundle.main`, because that is the app's bundle.

Your resources is in your framework's bundle.

Therefore the default main bundle that `NSLocalizedString` and `UIImage` use will fail in when you are developing a framework.

The correct way:

```swift
let frameworkBundle = Bundle(for: MyFrameworkClass.self)
let stringInFramework = NSLocalizedString(key, bundle: frameworkBundle, comment: "")
let imageInFramework = UIImage(named: "img", in: frameworkBundle, compatibleWith: nil)
```

`Bundle(for:)` conveniently find the bundle that contains the class. You could use any other class in your framework to replace `MyFrameworkClass`. It works because all code and resources will be in the same bundle for a framework.

## Playground

Playground is another special case.

Try running this in playground to see the `bundlePath` (the file URL path).

```swift
Bundle.main.bundlePath
Bundle(for: Framework1.self).bundlePath
Bundle(for: Framework2.self).bundlePath
```

The URL will look like this:

```
~/Library/Developer/XCPGDevices/x-x-x/data/Containers/Bundle/Application/y-y-y/UIPlayground-28059-1.app
~/Library/Developer/Xcode/DerivedData/App-xyz/Build/Products/Debug-iphonesimulator/Framework1.framework
~/Library/Developer/Xcode/DerivedData/App-xyz/Build/Products/Debug-iphonesimulator/Framework2.framework
```

If you add your resources to the "Resources" group in Playground, then the main bundle will contain the resources.

But if you need a resource from a framework, then you need the bundle for the framework.

## The Complicated Case

In our previous tutorial that [uses Playground to create application's UI](/2017/10/05/adding-playground-to-an-existing-project/), the scenario is complicated because the same resource is used in 2 targets:

1. The app
2. The framework for playground (`MyUIPlaygroundFramework`)

For such case, the resource has to specify the bundle correctly.

The trick is to make use of a class that **exists in both** the app and framework.

```swift
let bundle = Bundle(in: CommonClass.self)
```

When you use in app, `bundle` will be main bundle.

When you use in framework, `bundle` will be the framework's bundle.
