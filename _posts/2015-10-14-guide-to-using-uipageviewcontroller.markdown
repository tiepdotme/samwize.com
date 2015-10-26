---
layout: post
title: "Guide to Using UIPageViewController"
date: 2015-10-14T17:45:03+08:00
categories: [iOS]
---

In last post, we provided a guide on [how to create UIPageViewController in storyboard](/2015/10/13/how-to-create-uipageviewcontroller-in-storyboard-in-container-view/).

But we leave out quite a few things about [UIPageViewController](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIPageViewControllerClassReferenceClassRef/#//apple_ref/occ/instp/UIPageViewController/viewControllers).  This guide will cover some of these things.


## The Pages & ViewControllers

In our `UIPageViewController`, we declared `pages` to hold all our view controllers.

This `pages` is different from the provided property `viewControllers`.

- `pages` are all your view controllers
- `viewControllers` are the **current views** being displayed in the `UIPageViewController`. In iBook, 2 pages could be displayed together, and this property will hold the 2 view controllers.


## Currently Selected Index

Oddly, there is no property or event to let you know of the currently selected index.

As such, you have ride on `UIPageViewControllerDelegate` and manually track it.

```swift
// Track the current index
var currentIndex: Int?
private var pendingIndex: Int?

func pageViewController(pageViewController: UIPageViewController, willTransitionToViewControllers pendingViewControllers: [UIViewController]) {
    pendingIndex = pages.indexOf(pendingViewControllers.first!)
}

func pageViewController(pageViewController: UIPageViewController, didFinishAnimating finished: Bool, previousViewControllers: [UIViewController], transitionCompleted completed: Bool) {
    if completed {
        currentIndex = pendingIndex
    }
}
```


## Page Indicator

Page indicator is automatically provided if you use "scroll" transition, and you implement the 2 `UIPageViewControllerDataSource` methods below:

```swift
func presentationCountForPageViewController(pageViewController: UIPageViewController) -> Int {
    return pages.count
}

func presentationIndexForPageViewController(pageViewController: UIPageViewController) -> Int {
    return 0
}
```

You could be wondering why `return 0` for the index?

As the [doc](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPageViewControllerDataSourceProtocolRef/index.html#//apple_ref/occ/intfm/UIPageViewControllerDataSource/presentationIndexForPageViewController:) says:

> After gesture-driven navigation, these methods are not called. The index is updated automatically and the number of view controllers is expected to remain constant.

The method is never called when you swipe to scroll. It is called only when you call `setViewControllers:direction:animated:completion:` on the first time.

