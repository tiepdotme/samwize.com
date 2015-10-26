---
layout: post
title: "How to Create UIPageViewController in Storyboard"
date: 2015-10-13T12:37:57+08:00
categories: [iOS]
---

## Problem

I am creating a scene whereby in that 1 view controller, there will be multiple pages that shows different views.

User can swipe the pages, or use the segment to change the page.


## Solution

This design problem is pretty common, especially in an [app intro scene](http://www.appcoda.com/uipageviewcontroller-storyboard-tutorial/).

The solution is to construct the storyboard as such:

[{% img /images/uipageviewcontroller-storyboard.png "Storyboard Setup" %}](/images/uipageviewcontroller-storyboard.png)

- The scene has a container view
- The container view controller is replaced with a `UIPageViewController`
- The `UIPageViewController` has segues to it's pages (there are 2 pages here)
- The pages have the storyboard ID `page1` and `page2`


### UIPageViewController

NOTE: Firstly, the segue to the pages are actually not needed. I created the show segue so that it is clearer that the `UIPageViewController` has 2 children/pages.

This is a lacking feature of [UIPageViewController](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/PageViewControllers.html): it does not support segue to it's children, unlike `UINavigationController` or `UITabBarController`.

Instead, you can only link them up in code.

Here's the source code of the `UIPageViewController` class in swift:

```swift
import UIKit

class MyPageViewController: UIPageViewController, UIPageViewControllerDataSource, UIPageViewControllerDelegate {

    var pages = [UIViewController]()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.delegate = self
        self.dataSource = self

        let page1: UIViewController! = storyboard?.instantiateViewControllerWithIdentifier("page1")
        let page2: UIViewController! = storyboard?.instantiateViewControllerWithIdentifier("page2")
        
        pages.append(page1)
        pages.append(page2)
        
        setViewControllers([page1], direction: UIPageViewControllerNavigationDirection.Forward, animated: false, completion: nil)
    }

    func pageViewController(pageViewController: UIPageViewController, viewControllerBeforeViewController viewController: UIViewController) -> UIViewController? {
        let currentIndex = pages.indexOf(viewController)!
        let previousIndex = abs((currentIndex - 1) % pages.count)
        return pages[previousIndex]
    }
    
    func pageViewController(pageViewController: UIPageViewController, viewControllerAfterViewController viewController: UIViewController) -> UIViewController? {
        let currentIndex = pages.indexOf(viewController)!
        let nextIndex = abs((currentIndex + 1) % pages.count)
        return pages[nextIndex]
    }
    
    func presentationCountForPageViewController(pageViewController: UIPageViewController) -> Int {
        return pages.count
    }
    
    func presentationIndexForPageViewController(pageViewController: UIPageViewController) -> Int {
        return 0
    }
}
```

The code basically setup the view controllers for the pages by instantiating with the storyboard ID.

Then it implement the datasource that tells which view controllers are in the pages.

That's it. That's the only code needed.


### Container View

[Container View](https://github.com/codepath/ios_guides/wiki/Container-View-Controllers) is a very nice feature to manage complex scene with multiple views.

When you add a container view to a scene, another sub-scene will be added. This sub-scene is embedded into the main scene, and the size will be the same as the container view.

It is one of the coolest feature, introduced in iOS 5 that makes designing your storyboard much neater.

There's no code involved, just drag the container view.

Just note that in the main scene view controller, you can access `childViewControllers` to get all container view controllers. In our case, there is only 1 `UIPageViewController` as the container view controller.

