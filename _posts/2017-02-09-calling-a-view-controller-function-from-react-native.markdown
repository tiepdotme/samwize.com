---
layout: post
title: "Calling a View Controller Function From React Native"
date: 2017-02-09T14:02:35+08:00
categories: [React Native]
---

When you mix React Native with existing Swift/Objective-C code base, it gets complex for even the simplest of tasks.

[Instagram admits the challenge of a hybrid app](https://engineering.instagram.com/react-native-at-instagram-dd828a9a90c7).


## The Set Up

My view controller is written in **Swift**, and creates the `RCTRootView` in `viewDidLoad`. It managers the react native view.

I was trying to call a function I have in my view controller, from my JavaScript code.


## The Long Documentation, Again

React Native provided a long documentation separated across three pages:

- [Inter Communication](http://facebook.github.io/react-native/docs/communication-ios.html)
- [Native Modules](http://facebook.github.io/react-native/docs/native-modules-ios.html)
- [Native UI Components](http://facebook.github.io/react-native/docs/native-components-ios.html)

It wasn't clear nor simple.

Hope I can clear it up here (:


## The Thing about Swift

In the [very last section](http://facebook.github.io/react-native/docs/native-modules-ios.html#exporting-swift), the documentation says this about using Swift:

> Swift doesn't have support for macros (which React Native requires for exposing native modules and methods)

It is necessary that you have some Objective-C code because the macros provided only works with Objective-C.

Example: `RCT_EXTERN_MODULE` and `RCT_EXTERN_METHOD`

If you use Swfit, then you need to deal with using [bridging headers and stuff](https://developer.apple.com/library/content/documentation/Swift/Conceptual/BuildingCocoaApps/MixandMatch.html). Do not confuse this **Swift and Objective-C bridging** with **React Native and Objective-C bridging**. 

There are just many bridges to cross.


## How Bridging Works

In the [very beginning section](http://facebook.github.io/react-native/docs/native-modules-ios.html#ios-calendar-module-example), it gives a hint about how the bridge (to React native) happens:

> The CalendarManager module is instantiated on the Objective-C side using a [CalendarManager new] call. 

See how the JavaScript calls the method.

```javascript
import { NativeModules } from 'react-native';
var CalendarManager = NativeModules.CalendarManager;
CalendarManager.addEvent('Birthday Party', '4 Privet Drive, Surrey');
```

It _looks like_ the module is using a **singleton** pattern. It is NOT!

It is calling with a newly created object.

It is confirmed in the [section on Native UI Components](http://facebook.github.io/react-native/docs/native-components-ios.html#properties).

> Native views are created and manipulated by subclasses of RCTViewManager. These subclasses are similar in function to view controllers, but are essentially singletons - only one instance of each is created by the bridge.

And finally, in a [separate place](http://facebook.github.io/react-native/docs/communication-ios.html#calling-native-functions-from-react-native-native-modules), it warns the limitation when calling native functions from React Native (native modules):

> The fact that native modules are singletons limits the mechanism in context of embedding. 

If you still not clear what the problem entails.. let me explain with our view controller example.

We have an instance of our view controller. When JavaScript invokes a method (see `CalendarManager` example), it will use the singleton instance, not the actual instance! This is limiting because you need an identifier to the actual instance, using perhaps a number or string, then pass it to the singleton instance, for it to identiy the actual instance.

Still confused? Look at how it is implemented in code in my weak attempt #1.

## Attempt #1

Let's start with our view controller:

```swift
class AwesomeViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()

        AwesomeManager.viewController = self
        
        let rootView = RCTRootView(bridge: bridge, moduleName: "AwesomeScreen", initialProperties: nil)
        view = rootView
    }
    
    // The fun
    func foo(text: String) { ... }
}
```

We are using the view controller `viewDidLoad` to create the `RCTRootView` (init your bridge accordingly). 

The trick here is the part on `AwesomeManager.viewController = self`. This is where the view controller tell the "manager" that _you may control me_.

Let's look at the "manager" code, in Swift:

```swift
@objc(AwesomeManager)
class AwesomeManager: NSObject {
    
    static var viewController: AwesomeViewController?
    
    @objc func foo(text: String) {
        if let viewController = AwesomeManager.viewController {
            dispatch_async(dispatch_get_main_queue(), { () -> Void in
                viewController.foo("bar")
            })
        }
    }
    
}
```

## Attempt #2 ?

Ah, I'm exhausted. Gonna leave this till next time (:

You may attempt and comment below?
