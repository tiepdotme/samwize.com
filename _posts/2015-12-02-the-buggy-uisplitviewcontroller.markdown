---
layout: post
title: "The Buggy UISplitViewController"
date: 2015-12-02T18:18:54+08:00
categories: [iOS]
---

When iOS 8 was released, `UISplitViewController` has a big upgrade -- it is no longer restricted to only iPad. That means, you can use the same on iPhone, therefore building a truly universal app.

NSHipster has a [good guide](https://www.google.com/search?client=safari&rls=en&q=uisplitviewcontroller+bug&ie=UTF-8&oe=UTF-8) to `UISplitViewController`.

Here, I only want to point out the horrible bugs.



## My Typical Setup

{%img /images/uisplitviewcontroller-setup.png %}

My storyboard consists of:

1. a master/primary view in a `UINavigationViewController`
2. a detail/secondary view in a `UINavigationViewController`
3. both have a `UITextField` pinned to the bottom

Not much code needed to demo the bugs. It's all in the master `ViewController` as such:

```swift
class ViewController: UIViewController, UISplitViewControllerDelegate {

    override func viewDidLoad() {
        super.viewDidLoad()

        splitViewController?.delegate = self

        // Display mode can be: AllVisible | PrimaryOverlay | etc
        // Set to .PrimaryOverlay, and the bugs will come
        self.splitViewController?.preferredDisplayMode = .PrimaryOverlay
    }

    func splitViewController(splitViewController: UISplitViewController, collapseSecondaryViewController secondaryViewController: UIViewController, ontoPrimaryViewController primaryViewController: UIViewController) -> Bool {
        // To show detail first for eg iPhone compact size
        return false
    }
}
```

I tested in simulator and real devices, running the latest iOS 9.1.


## The BUGS

When `preferredDisplayMode` is set to `PrimaryOverlay` (or `PrimaryHidden` or `Automatic`), 2 bugs will be introduced:

1. In iPad, swipe from left edge on detail will not show master!
2. The controller view will auto adjust whenever keyboard is shown

<iframe width="640" height="480" src="https://www.youtube.com/embed/vYGw9BCdyC4?rel=0" frameborder="0" allowfullscreen></iframe><br/>


In fact, only by setting to `AllVisible` then things work correctly.

Bug #1 - Not being able to swipe from left edge is irritating, but luckily you can still have `displayModeButtonItem` that can be added to your navigation bar.

Bug #2 - auto adjusting your view is much more irritating. It looks like a _feature_ at first, because it will prevent keyboard from blocking your `UITextField`.

However, the behaviour is buggy in iPhone. By switching to landscape, then back to portrait (as shown in the video above), the view will not auto adjust now! Because of that, you can't safely assume your view will be adjusted correctly. Neither can you add codes to prevent the keyboard from blocking other views.

What's more, the animation is laggy..

Terribe. Terrible.



## Bonus Bug

Actually, there could be a warning `Unbalanced calls to begin/end appearance transitions` if you simply set `preferredDisplayMode`.

You have to wrap it around a dispatch block..

```swift
        // Why set the display mode in dispatch?
        // It's a workaround: http://stackoverflow.com/a/28440974/242682
        dispatch_async(dispatch_get_main_queue()) {
            // Display mode can be: AllVisible | PrimaryOverlay | etc
            // The bugs coming from the mode .PrimaryOverlay
            self.splitViewController?.preferredDisplayMode = .PrimaryOverlay
        }
```

It's a warning, but show how buggy `UISplitViewController` is!



## UPDATE

After conversing with an Apple engineer, here is what I knew.

`preferredDisplayMode` is merely _your preferred mode_. The actual mode depends on the device size class, as [stated in doc](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UISplitViewController_class/#//apple_ref/occ/instp/UISplitViewController/preferredDisplayMode).

> Use this property to specify the display mode that you prefer to use. The split view controller makes every effort to adopt the interface you specify but may use a different type of interface if there is not enough space to support your preferred choice. If changing the value of this property leads to an actual change in the current display mode, the split view controller animates the resulting change.

> Setting the value of this property to `UISplitViewControllerDisplayModeAutomatic` causes the split view controller to choose the most appropriate display mode for the currently available space. On iPad, this results in use of the mode `UISplitViewControllerDisplayModePrimaryOverlay` in portrait orientations and `UISplitViewControllerDisplayModeAllVisible` in landscape orientations. The default value of this property is `UISplitViewControllerDisplayModeAutomatic`.

When you rotate to landscape (regular horizontally), the mode will change to `AllVisible` automatically. And even after you rotate to portrait, it will remain `AllVisible`!

Hence, if you want it to be always `PrimaryOverlay`, then you have to set `preferredDisplayMode` every time there is a rotation:

```swift
    override func willAnimateRotationToInterfaceOrientation(toInterfaceOrientation: UIInterfaceOrientation, duration: NSTimeInterval) {
        self.splitViewController?.preferredDisplayMode = .PrimaryOverlay
    }
```

Okay, got that.

Now the last mystery is why would `PrimaryOverlay` cause the view to adjust when keyboard is shown?

This is because in `PrimaryOverlay`, the master view is presented within a **popover**, and popovers will automatically adjust in response to keyboard..
