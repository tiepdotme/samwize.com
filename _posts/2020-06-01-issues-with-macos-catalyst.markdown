---
layout: post
title: "Issues with macOS Catalyst"
date: 2020-06-01T17:11:09+08:00
categories: [macOS]
---

While working on an iOS app with SwiftUI, I thought I should trying converting it to a Catalyst app.

## Just 1 checkbox to enable

It's easy. In **Deployment Info**, enable Mac.

It's _easy_, but there're couple of issues I faced.

## Provisioning Profiles

I use fastlane match, and it ran into an issue. My app uses App Group, and the profiles would never include the capability. Apple developer portal does not have the option to add the capability for macOS Catalyst, somehow.

The solution: Let Xcode manage, and it will be fine 🤷🏻‍♂️

## Same App ID as iOS?

The option **Use iOS Bundle Identifier** has implications.

- Enabled: [Universal Purchase](https://developer.apple.com/support/universal-purchase/), 1 app, multiple platforms
- Disabled: Use different ID, different apps on App Store Connect

Most likely you want it enabled so that it is 1 app family. Pay once, use anywhere.

## Cocoapods

I use Cocoapods, and also SPM.

For Cocoapods, if any of the frameworks is unsupported (eg. Fabric), they have to be excluded for macOS Catalyst.

There is good solution [by fermoya](https://medium.com/better-programming/macos-catalyst-debugging-problems-using-catalyst-and-cocoapods-579679150fa9) to not compile those pods. The ruby script he provided is [here](https://gist.github.com/fermoya/f9be855ad040d5545ae3cb254ed201e4#file-remove_unsupported_libraries-rb), and it works like this in Podfile:

```ruby
load 'remove_unsupported_libraries.rb'

def unsupported_pods
   ['Fabric', 'Crashlytics', 'Firebase']
end

def supported_pods
    ['IQKeyboardManagerSwift']
end

post_install do |installer|
   installer.configure_support_catalyst(supported_pods, unsupported_pods)
end
```

## Preprocessor Code

Even after you exclude unsupported frameworks, you will still need to write some preprocessor codes like this:

```swift
#if !targetEnvironment(macCatalyst)
import Firebase
import Crashlytics
#endif
```

Or you might be using `#available`:

```swift
if #available(macCatalyst 13.4, *) {
    ...
}
```

NOTE: `#available` is not a preprocessor code. If you try to use it in a `ViewBuilder` (eg Group, VStack), it will have error that _the closure can't be used in a ViewBuilder_.

## Final thoughts

Catalyst is very new technology. There's still lots to improve on. SwiftUI has so much limitations and bugs!

But it's a great feeling when you can write once and it runs on iPhone, iPad, and macOS.

![](/images/mac-catalyst-the-king.jpg)
