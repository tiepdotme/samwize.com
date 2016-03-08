---
layout: post
title: "Using UIPageViewController With Custom UIPageControl"
date: 2016-03-08T16:10:42+08:00
categories: [iOS]
---

I have [previously](http://samwize.com/2015/10/13/how-to-create-uipageviewcontroller-in-storyboard-in-container-view/) [provided](http://samwize.com/2015/10/14/guide-to-using-uipageviewcontroller/) 2 posts about `UIPageViewController`, but that's not enough.

A very common problem with `UIPageViewController` is that you [cannot customize the UIPageControl](http://stackoverflow.com/q/17466718/242682).

Or should I say, it is not straight forward.


## The Trick

The trick is to use a custom `UIPageControl`.

You can drag one to your storyboard, link to your view controller. You will see this as `pageControl` in `WalkthroughViewController` below.

As you can infer from the view controller class name, an app walkthrough is a very good example where you want to have pages of tutorials.

In this implementation, I will also subclass my view controller with `UIViewController` (NOT `UIPageViewController`).

Therefore, I will create the `UIPageViewController` in code.


## The Code

```swift
class WalkthroughViewController: UIViewController, UIPageViewControllerDataSource, UIPageViewControllerDelegate {

    // The custom UIPageControl
    @IBOutlet weak var pageControl: UIPageControl!

    // The UIPageViewController
    var pageContainer: UIPageViewController!

    // The pages it contains
    var pages = [UIViewController]()

    // Track the current index
    var currentIndex: Int?
    private var pendingIndex: Int?

    // MARK: -

    override func viewDidLoad() {
        super.viewDidLoad()

        // Setup the pages
        let storyboard = UIStoryboard(name: "Walkthrough", bundle: nil)
        let page1: UIViewController! = storyboard.instantiateViewControllerWithIdentifier("page1")
        let page2: UIViewController! = storyboard.instantiateViewControllerWithIdentifier("page2")
        let page3: UIViewController! = storyboard.instantiateViewControllerWithIdentifier("page3")
        pages.append(page1)
        pages.append(page2)
        pages.append(page3)

        // Create the page container
        pageContainer = UIPageViewController(transitionStyle: .Scroll, navigationOrientation: .Horizontal, options: nil)
        pageContainer.delegate = self
        pageContainer.dataSource = self
        pageContainer.setViewControllers([page1], direction: UIPageViewControllerNavigationDirection.Forward, animated: false, completion: nil)

        // Add it to the view
        view.addSubview(pageContainer.view)

        // Configure our custom pageControl
        view.bringSubviewToFront(pageControl)
        pageControl.numberOfPages = pages.count
        pageControl.currentPage = 0
    }

    // MARK: - UIPageViewController delegates

    func pageViewController(pageViewController: UIPageViewController, viewControllerBeforeViewController viewController: UIViewController) -> UIViewController? {
        let currentIndex = pages.indexOf(viewController)!
        if currentIndex == 0 {
            return nil
        }
        let previousIndex = abs((currentIndex - 1) % pages.count)
        return pages[previousIndex]
    }

    func pageViewController(pageViewController: UIPageViewController, viewControllerAfterViewController viewController: UIViewController) -> UIViewController? {
        let currentIndex = pages.indexOf(viewController)!
        if currentIndex == pages.count-1 {
            return nil
        }
        let nextIndex = abs((currentIndex + 1) % pages.count)
        return pages[nextIndex]
    }

    func pageViewController(pageViewController: UIPageViewController, willTransitionToViewControllers pendingViewControllers: [UIViewController]) {
        pendingIndex = pages.indexOf(pendingViewControllers.first!)
    }

    func pageViewController(pageViewController: UIPageViewController, didFinishAnimating finished: Bool, previousViewControllers: [UIViewController], transitionCompleted completed: Bool) {
        if completed {
            currentIndex = pendingIndex
            if let index = currentIndex {
                pageControl.currentPage = index
            }
        }
    }
}
```


## The Code to Remove

As important, you have to remove/comment these 2 methods, so that the default `UIPageControl` from the `UIPageViewController` is not shown.

```swift
//    func presentationCountForPageViewController(pageViewController: UIPageViewController) -> Int {
//        return pages.count
//    }
//    
//    func presentationIndexForPageViewController(pageViewController: UIPageViewController) -> Int {
//        return 0
//    }
```