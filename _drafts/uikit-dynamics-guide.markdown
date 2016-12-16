---
layout: post
title: "UIKit Dynamics Guide"
date: 2016-11-14T10:49:03+08:00
categories: [iOS, UIKit]
---

UIKit Dynamics was unveiled in [WWDC 2013](https://developer.apple.com/videos/play/wwdc2013/206/), for iOS 7.

The goal was to make it easy to create animated views and transitions.


## What about Core Animation, UIView Animation, etc

We already can animate views using various techniques, such as the popular [animate(withDuration:animations:)](https://developer.apple.com/reference/uikit/uiview/1622418-animate).

So why need UIKit Dynamics?

In short, UIKit Dynamics is a better framework.

> A composable, reusable, declarative, real-world inspired animation, and interaction system.

It is Physics inspired. Yet it is not Sprite Kit.

UI Dynamics _is like_ Sprite Kit, for non-gaming apps.


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
3. View
  - They are actually UIDynamicItem
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

  
## [UIDynamicItem](https://developer.apple.com/reference/uikit/uidynamicitem) is a Protocol

Provides UIKit the information it needs in order to animate an item. 

UIView implements it. 

You can implement it too.


## UICollectionView

`UICollectionViewLayoutAttributes` also implements `UIDynamicItem`.

objc.io has a good tutorial on [UICollectionView with dynamic animator](https://www.objc.io/issues/5-ios7/collection-views-and-uidynamics/), with an the [source](https://github.com/ashfurrow/ASHSpringyCollectionView/blob/master/ASHSpringyCollectionView/ASHSpringyCollectionViewFlowLayout.m) in github.


## More Resources

[WWDC 2015](https://developer.apple.com/videos/play/wwdc2015/229/) covered what's new.
