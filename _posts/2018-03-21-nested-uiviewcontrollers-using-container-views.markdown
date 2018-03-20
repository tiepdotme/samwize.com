---
layout: post
title: "Nested UIViewControllers Using Container"
date: 2018-03-21T16:33:12+08:00
categories: [UI]
---

[Container view controller](https://developer.apple.com/library/content/featuredarticles/ViewControllerPGforiPhoneOS/ImplementingaContainerViewController.html) is a new concept from iOS 8.

Yet prior to iOS 8, we have always been using containers, just that we didn't know, and it is not exposed publicly.

`UINavigationController`, `UITabBarController`, and `UISplitViewController` -- they are all container view controllers!

## What is a container?

> A container view controller embeds the content of other view controllers into its own root view. A container view controller may mix custom views with the contents of its child view controllers to facilitate navigation or to create unique interfaces.

For example, a `UINavigationController` manages a navigation bar (a custom view) and a stack of child view controllers (only one of which is visible at a time), and provides an API to add and remove child view controllers from the stack.

Your custom container is a **parent** (aka root) view controller and will be managing it's **children** view controllers. It is up to the container to render the whole view.

## When should you use Container View Controllers?

It is not entirely clear when developers should use, or not use.

Remember: Container is a technique for managing multiple view controllers, usually handling the navigation and the transition effect. It is _entirely possible to not use_ custom container, and we obviously could not use it prior to iOS 8.

What is an alternative?

An alternative is to manage multiple views (not view controllers).

When possible, you should NOT use container, if you are using simple views (not full blown view controller). _[Defines UIView vs UIViewController](https://stackoverflow.com/a/5789009/242682)._

### Scenario: Nested View Controllers

Let's discuss on a very common scenario where we use container to simple nest other view controllers.

If you use Xcode interface builder, you can easily drag a container view to the storyboard, and automatically a child view controller will be added onto the storyboard.

![Drag and drop 2 container views](/images/xcode-container-nested.png)

Using container in this way is simply to nest multiple view controllers in a parent view controller.

Note: "Child A" and "Child B" are simply `UIView`, aka container view, defined as:

> A region of a view controller that can include a child view controller

There is a benefit to using container in a storyboard. In the screenshot, "Child B" is actually right on top and obscuring "Child A".

Yet, because of container, you can see and design the individual child view controllers!

But, if you are coming from _no-storyboard pure coding way_, then using container and nested view controllers will not be an apparent solution. Because with code, you don't have problem of using interface builder, and you are used to creating custom views, even if they overlap.

I prefer the way of no-storyboard, so for the scenario of simply nesting multiple views, you should not use container.

### Checklist to using container

If you answer YES to this checklist, then go ahead and use container.

- Child view controller is independent to the parent
- Child view controller is equivalent to a screen
- Child view controller determines the status bar style
- Child view controller requires `viewDidAppear` etc events
- Child is really a view controller and not just a view
- Parent view controller navigate (push/popping/mixing) the child view controllers
- Parent view controller controls the transition effects when navigating
- Parent view controller probably showing 1 or 2 child at 1 time

If not, fallback to regular views.

## Adding a child view controller

You need [these steps](https://developer.apple.com/library/content/featuredarticles/ViewControllerPGforiPhoneOS/ImplementingaContainerViewController.html#//apple_ref/doc/uid/TP40007457-CH11-SW13):

1. Container view controller to call `addChildViewController`, so that UIKit knows your container is now managing the child view controller.

2. Add the child’s view to your container’s view hierarchy.

3. Setup auto layout constraints for the child's view.

4. Child view controller to call `didMoveToParentViewController`.

```swift
// Creating the child in Parent's viewDidLoad
override func viewDidLoad() {
    let child = MyChildViewController()
    addChildViewController(child)
    view.addSubview(child.view)
    // Setup auto layout constraints for child.view..
    child.didMove(toParentViewController: self)
}
```

To remove a child view controller, you undo the with the corresponding methods.
