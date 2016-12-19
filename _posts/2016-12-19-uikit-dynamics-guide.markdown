---
layout: post
title: "UIKit Dynamics Guide"
date: 2016-12-19T16:49:03+08:00
categories: [iOS, UIKit]
---

UIKit Dynamics was unveiled in [WWDC 2013](https://developer.apple.com/videos/play/wwdc2013/206/), for iOS 7.

The goal was to make it easy to create animated views and transitions, based on physics.


## What about Core Animation, UIView Animation, etc

We already can animate views using various techniques, such as:

1. the popular [animate(withDuration:animations:)](https://developer.apple.com/reference/uikit/uiview/1622418-animate), and

2. [Core Animation](/2016/12/16/core-animation-guide/), which we covered earlier.

So why need UIKit Dynamics?

In short, UIKit Dynamics is a better, physics-inspired framework.

> A composable, reusable, declarative, real-world inspired animation, and interaction system.

UI Dynamics _is like_ Sprite Kit, but for non-gaming apps.

(Note: Core Animation is great, but it is for pure animation. It has no "physics engine", no collision detection and sort. So choose the framework you need.)


## Architecture

![UIKit Dynamic Architecture Diagram](/images/uikit-dynamic-architecture.png)

1. UIDynamicAnimator
  - Has a reference view (think of it as a canvas)
  - Provide overall context 
  - Keep track of all the behavioiurs
2. UIDynamicBehavior
  - Declarative
  - Composable
  - Configure the parameters then add to animator
3. UIDynamicItem/View
  - [UIDynamicItem](https://developer.apple.com/reference/uikit/uidynamicitem) is a protocol, providing UIKit the information it needs to animate an item
  - `UIView` implements it, but you can implement it too
  - Behaviour-View is n-n


## Primitive Behaviours

You can create/compose your own `UIDynamicBehavior`, but these common ones are provided out of the box:

- Gravity
  - ~~Earth Gravity~~ UI Kit Gravity = 1000 point/sec^2
- Collision
  - Between items or boundary
- Attachment
  - Spring
- Snap
  - Snap in place
- Push
  - ~~Newton Force~~ UIKit Newton = Accelerate (100,100) to 100 point/sec^2
  - Continuous or instantenous
- UIDynamicItemBehavior
  - Item-level properties: friction, elasticiy, density, etc


## The Swift Code

This is how you create a simple physics behaviour (actually made up of 3 `UIDynamicBehavior`) of an image view falling through gravity and bouncing off the container view:

```swift
// The view controller must hold on to the animator object
var animator: UIDynamicAnimator!

override func viewDidLoad() {
    super.viewDidLoad()

    // Create the animator
    animator = UIDynamicAnimator(referenceView: view)

    // Create behaviour #1 - Gravity
    let gravity = UIGravityBehavior(items: [imageView])
    animator.addBehavior(gravity)

    // Create behaviour #2 - Collision
    let collision = UICollisionBehavior(items: [imageView])
    collision.translatesReferenceBoundsIntoBoundary = true
    animator.addBehavior(collision)

    // Create behaviour #3 - Elasticity etc
    let behaviour = UIDynamicItemBehavior(items: [imageView])
    behaviour.allowsRotation = false
    behaviour.elasticity = 0.5
    animator.addBehavior(behaviour)
}
```


## With Autolayout

Autolayout is _incompatitble_ with animation - using UIKit Dynamics or Core Animation.

In [Core Animation](/2016/12/16/core-animation-guide/), the framework works on the **presentation** layer, while the actual **model** has to be explicitly updated when the animation is completed.

This is the same for UIKit Dynamics.

Use the animator's [delegate](https://developer.apple.com/reference/uikit/uidynamicanimatordelegate#//apple_ref/occ/intfm/UIDynamicAnimatorDelegate/dynamicAnimatorDidPause) to know when the animation is completed, and update your autolayout constraints to the final state..

```swift
// In `viewDidLoad`, set the delegate
animator.delegate = self
  
// UIDynamicAnimatorDelegate
func dynamicAnimatorDidPause(animator: UIDynamicAnimator) {
    // Update the constraint to the final state
}
```

## UICollectionView

`UICollectionViewLayoutAttributes` also implements `UIDynamicItem`.

This provides some cool animation to all the items in a collection view.

objc.io has a good tutorial on [UICollectionView with dynamic animator](https://www.objc.io/issues/5-ios7/collection-views-and-uidynamics/), with the [source](https://github.com/ashfurrow/ASHSpringyCollectionView/blob/master/ASHSpringyCollectionView/ASHSpringyCollectionViewFlowLayout.m) in github.
