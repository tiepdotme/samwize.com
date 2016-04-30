---
layout: post
title: "Making Tab Bar Slide When Selected"
date: 2016-04-27T15:55:49+08:00
categories: [iOS]
---

This is a Swift code snippet for making the tab bar have a sliding animation when selected.

This post takes reference from the [answers](http://stackoverflow.com/a/5180104/242682) on StackOverflow. 

The difference:

- Code in Swift
- With a private method `animateToTab` which you can call programatically
- Less code using `frame.center` etc


## The Code

Hooking up to `UITabBarControllerDelegate`, you can start the animation when a tab is about to be selected:

```swift
func tabBarController(tabBarController: UITabBarController, shouldSelectViewController viewController: UIViewController) -> Bool {
    let tabViewControllers = tabBarController.viewControllers!
    guard let toIndex = tabViewControllers.indexOf(viewController) else {
        return false
    }
    
    // Our method
    animateToTab(toIndex)
    
    return true
}
```

We have our method `animateToTab`, which contains the bulk of the code for animating the views.

Remember: You can even call `animateToTab` programmatically to switch tab, with the same sliding animation.

```swift
func animateToTab(toIndex: Int) {
    let tabViewControllers = viewControllers!
    let fromView = selectedViewController!.view
    let toView = tabViewControllers[toIndex].view    
    let fromIndex = tabViewControllers.indexOf(selectedViewController!)
    
    guard fromIndex != toIndex else {return}
    
    // Add the toView to the tab bar view
    fromView.superview!.addSubview(toView)
    
    // Position toView off screen (to the left/right of fromView)
    let screenWidth = UIScreen.mainScreen().bounds.size.width;
    let scrollRight = toIndex > fromIndex;
    let offset = (scrollRight ? screenWidth : -screenWidth)
    toView.center = CGPoint(x: fromView.center.x + offset, y: toView.center.y)

    // Disable interaction during animation
    view.userInteractionEnabled = false
    
    UIView.animateWithDuration(0.5, delay: 0.0, usingSpringWithDamping: 1, initialSpringVelocity: 0, options: UIViewAnimationOptions.CurveEaseOut, animations: {
        
            // Slide the views by -offset
            fromView.center = CGPoint(x: fromView.center.x - offset, y: fromView.center.y);
            toView.center   = CGPoint(x: toView.center.x - offset, y: toView.center.y);
            
        }, completion: { finished in
            
            // Remove the old view from the tabbar view.
            fromView.removeFromSuperview()
            self.selectedIndex = toIndex
            self.view.userInteractionEnabled = true
        })
}
```

## A Lesson Learnt

In [one of the answers](http://stackoverflow.com/a/30620627/242682) (also swift code), the author uses CA transformation, instead of changing the frames.

That doesn't work in some cases eg. triggered `animateToTab` programmatically

I suspect it is the same as the [embarrassing problem](http://blog.spacemanlabs.com/2012/08/premature-completion-an-embarrassing-problem/) whereby the animation completed prematuring, because iOS "don't see any animation" to do. In the completion block, `finished` will be false, which would mean the animation wasn't completed.

I do not know exactly how to fix the code, but changing to frames work. 

Do you know what's wrong?
