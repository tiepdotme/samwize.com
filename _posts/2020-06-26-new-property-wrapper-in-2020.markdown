---
layout: post
title: "New Property Wrapper in 2020"
date: 2020-06-26T17:34:58+08:00
categories: []
---

WWDC 2020 introduced a few more property wrappers. [I don't take system wrappers lightly](/2019/12/04/guide-to-property-wrapper/), as they must be introduced for very good reason. So better know them well.

They are available in iOS 14, macOS 11 (aka 10.16), tvOS 14, and watchOS 7. _Let's stick with just the year._

Some of the new 2020 APIs: [AppStorage](https://developer.apple.com/documentation/swiftui/appstorage), [SceneStorage](https://developer.apple.com/documentation/swiftui/scenestorage), [ScaledMetric](https://developer.apple.com/documentation/swiftui/scaledmetric), [StateObject](https://developer.apple.com/documentation/swiftui/stateobject), [UIApplicationDelegateAdaptor](https://developer.apple.com/documentation/swiftui/uiapplicationdelegateadaptor).

## @AppStorage for UserDefaults

AppStorage is the **persistent storage** provided by SwiftUI, with `UserDefaults` as the underlying backing data. The following code will persist the email across launched.

```swift
struct AppStorageView: View {
    @AppStorage("email") var email = "initial@hey.com"
    var body: some View {
        TextField("Email Address", text: $email)
    }
}
```

Any change to the underlying `UserDefaults` will _publish_ the changes, therefore updating the SwiftUI view.

## @SceneStorage for scenes

It is another persistent storage for SwiftUI, but for **each scene state restoration**. You're not familiar with scene lifecycle, it is introduced in [2019](https://developer.apple.com/videos/play/wwdc2019/212/) for multi-windows.

Unlike AppStorage,

> The underlying data that backs SceneStorage is not available to you

NOTE: This can only be used with [SwiftUI App + Scene structure](https://developer.apple.com/documentation/swiftui/app-structure-and-behavior). If you're using the _old_ AppDelegate way, there will be fatal error "@SceneStorage is only for use with SwiftUI App Lifecycle".

## @StateObject

Initiating with `@ObservedObject` in a view is [**unsafe**](https://developer.apple.com/documentation/swiftui/managing-model-data-in-your-app).

```swift
struct MyView: View {
  @ObservedObject var myModel = MyModel() // UNSAFE
}
```

> SwiftUI might create or recreate a view at any time, so it’s important that initializing a view with a given set of inputs always results in the same view. As a result, it’s unsafe to create an observed object inside a view.

Instead, use `@StateObject` for a view (or structure), **when it can be the owner**.

For external dependencies, stick to `@ObservedObject` and also `@EnvironmentObject`.

```swift
struct MyView: View {
  @StateObject var myModel = MyModel()
  @ObservedObject var externalModel: ExternalModel
  @EnvironmentObject var globalModel: GlobalModel
}
```

You can also create `StateObject` in `App` or `Scene` -- if they can be the owner and should hold on to the truth.

```swift
struct MyApp: App { // Or for Scene
    @StateObject var store = Store()
}
```

## @ScaledMetric

This scales a float according to the user's Dynamic Type settings. All along, only fonts are scaled automatically. Now we can scale an image like this:

```swift
@ScaledMetric var length: CGFloat = 100
var body: some View {
    Image(systemName: "bolt.fill")
        .resizable()
        .frame(width: length, height: length)
}
```

## @UIApplicationDelegateAdaptor

If you are mixing UIKit's AppDelegate and SwiftUI, then this is a way to access the app delegate. I don't see any reason why this is even needed, but here's the gist:

```swift
@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {
    func foo() { print("foo") }
}

struct MyView: View {
    @UIApplicationDelegateAdaptor(AppDelegate.self) var appDelegate
    var body: some View {
        Button("Foo") {
            appDelegate.foo()
        }
    }
}
```
