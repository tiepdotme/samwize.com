---
layout: post
title: "Automate Screenshots Capture Using Snapshot (via Xcode UI Testing)"
date: 2015-12-09T13:44:13+08:00
categories: [iOS, Tests]
---

Tools are [evolving](http://samwize.com/2015/01/29/fastlane-replacing-ui-screen-shooter-and-screenshot-uploader/).

Fastlane [snapshot](https://github.com/fastlane/snapshot) now uses Xcode 7 most awesome feature - UI Testing!


## Xcode UI Testing

If you are not familiar what this awesome feature from Xcode 7 is, then watch [WWDC 2015](https://developer.apple.com/videos/play/wwdc2015-406/) for a demo.

In short, developers can now:

- record UI interactions
- replay and test
- view test report which includes screenshots at every stage
- view test report which includes code coverage too


## How to use

There is a good [UI Testing Cheat Sheet and Example](http://masilotti.com/ui-testing-cheat-sheet/) from masilotti.

More (and lengthy) details from [Apple Doc](https://developer.apple.com/library/prerelease/ios/documentation/DeveloperTools/Conceptual/testing_with_xcode/chapters/01-introduction.html).


## Using with snapshot

Setting up snapshot with a UI Test target is easy. The few steps are provided in it's [quick guide](https://github.com/fastlane/snapshot).

What snapshot did not point (perhaps obvious) is this:

Snapshot will run **every test case** in the UI test target. Therefore, you should have **2 UI test targets**:

1. For full UI testing (if you need)
2. For snapshot to capture screenshots

The test target for snapshot would suffice with 1 test case that walk through the important screens that you need to capture. Maybe 1, if you support only iPhone and not iPad/etc.


## Testing iPad vs iPhone

Another caveat is that snapshot will run through all devices you specified in your `Snapfile`.

That means, your test case could fail if your iPhone interface is different from iPad interface.

To workaround, you should have 2 test cases in snapshot test target:

```swift
func testSnapshotPhone() {
    guard UIDevice.currentDevice().userInterfaceIdiom == .Phone else { return }
    // Capture screenshots for iPhone
}

func testSnapshotPad() {
    guard UIDevice.currentDevice().userInterfaceIdiom == .Pad else { return }
    // Capture screenshots for iPad
}
```


## Prefill Database

A common requirement when taking screenshots is to have a prefill database so that the screenshots are consistent.

You might think that can you clean up Core Data, then manually add objects in the test case `setUp()` function. That won't work.

This is because the test target is _not the same_ as app target.

You could use a backdoor, or simply provide a logic in your app to know when it is [run via snapshot](https://github.com/fastlane/snapshot#launch-arguments):

```swift
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
    if NSUserDefaults.standardUserDefaults().boolForKey("FASTLANE_SNAPSHOT") {
        // Prefill database
    }
}
```



