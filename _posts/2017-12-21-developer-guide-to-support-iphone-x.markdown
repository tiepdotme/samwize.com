---
layout: post
title: "Developer Guide to Support iPhone X"
date: 2017-12-21T11:49:22+08:00
categories: [iOS]
---

iPhone X introduced The Notch, The Home Indicator, Safe Area, and a different resolution form factor..

UI design is much affected.

This post is a guide walking through various aspect and how it affects development.

Let's start with the biggest change in the phone.

## The Notch 

I like [maxrudberg's mentality](http://blog.maxrudberg.com/post/166045445103/ui-design-for-iphone-x-top-elements-and-the-notch) towards The Notch:

> Eventually, they will get rid of the notch. It could be 2, 5, or even 10 years, but it’s a stop gap, not a permanent design solution. In the meantime, treat it like the elephant in the room. We all know it’s there, but for the most part, you should design as if it’s not.

Apple's Music app and etc uses Card style, and that is against their guideline towards the top notch space. 

So, don't always follow guidelines.

## The Home Indicator

Unlike The Notch, the home indicator is a software element. It is a UI on the screen, at the bottom. 

To allow auto hide (after user not touching screen) for a view controller:

```swift
override func prefersHomeIndicatorAutoHidden() -> Bool {
    return true
}
```

But [note](https://medium.com/the-traveled-ios-developers-guide/iphone-x-dealing-with-home-indicator-2e8e47f5647f) the comment in the doc:

> The system takes your preference into account, but returning YES is not a guarantee that an indicator will be hidden.

## Nav bar

iOS automatically fills a nav bar background for the extra space at the top.

Use **black** and it blends with the Notch.

Use **transparent** and extend your content view beyond.

## Status Bar when you have Nav Bar

There is an [undocumented behaviour](https://forums.developer.apple.com/thread/88962):

> If you want to hide the status bar on iPhone X, you should also hide the navigation bar, otherwise you should leave both visible. This is the behavior that UINavigationController implements.

If you implemented [`prefersStatusBarHidden`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621440-prefersstatusbarhidden) or [`childViewControllerForStatusBarHidden`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621451-childviewcontrollerforstatusbarh), they will NOT work as intended, if you have the navigation bar shown.

Status bar will always be shown, if navigation bar is shown. 

You may only **control the color** of the status bar. 

~~This [method is weird](http://stackoverflow.com/a/19513714/242682), but yes it works.~~ 

This [extension](https://stackoverflow.com/a/42301499/242682) to `UINavigationController` is the cleanest solution (another alternative is subclass it).

```swift
extension UINavigationController {
    override open var childViewControllerForStatusBarStyle: UIViewController? {
        return self.topViewController
    }
}
```

## Table View

`UITableView` should still layout edge to edge (NOT to safe area).

Instead, the **content inset** should be mark safe via [`insetsContentViewsToSafeArea`](https://developer.apple.com/documentation/uikit/uitableview/2921665-insetscontentviewstosafearea) (new in iOS 11).

```swift
if #available(iOS 11.0, *) {
    tableView.insetsContentViewsToSafeArea = true
}
```

## Scroll View

[`contentInsetAdjustmentBehavior`](https://developer.apple.com/documentation/uikit/uiscrollview/2902261-contentinsetadjustmentbehavior) provides more control on how safe area modifies the content inset.

## Search Controller

iOS seems to be not adjusting correctly for search controller. Fixed with:

```swift
tableView.contentInsetAdjustmentBehavior = .never
```

[Search controller](/2016/11/27/uisearchcontroller-development-guide/) is a difficult topic, and with iPhone X the recommended way is to set the search bar in the nav bar.

## Storyboard

If you use storyboard, in File Inspector, check **Use Safe Area Layout Guides**, and interface builder will replace the old top bottom layout guides with the new safe area.

After which, it is up to you to make changes depending on your UI.

Some pointers:

- For table view, don't **Clip to Bounds** (under Attributes Inspector)
- For cutom bottom tool bar, you can provide a background for the void occupied by Home Indicator

There are more guides. [Apple official guide](https://developer.apple.com/ios/update-apps-for-iphone-x/), and [many](https://useyourloaf.com/blog/supporting-iphone-x/) [more](https://medium.com/rosberryapps/ios-safe-area-ca10e919526f). [Designcode.io](https://designcode.io/ios11-iphone-x) summed up well, with many valuable resources for design. [Sketch file](https://iosdesignkit.io/ios-11-gui/) & [device mockups](http://v1.designcode.io/angle).
