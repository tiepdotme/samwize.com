---
layout: post
title: "Core Animation Guide"
date: 2016-12-16T16:30:39+08:00
categories: [iOS]
---

There are many ways to perform animation, and using [Core Animation](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CoreAnimation_guide/CreatingBasicAnimations/CreatingBasicAnimations.html) is one great framework provided by Apple.

Core Animation operates on `CALayer`, which is the `layer` property that each `UIView` has. It is a presentation layer.


## Basic animation

Let's jump right into the code to "tell" a view to move right by 50 pt:

```swift
let move = CABasicAnimation()
move.keyPath = "position.x"
move.byValue = 50   // Move right by 50 pt
move.duration = 1   // 1 second
move.timingFunction = CAMediaTimingFunction(name: kCAMediaTimingFunctionEaseInEaseOut)
move.fillMode = kCAFillModeForwards
move.removedOnCompletion = false
view.layer.addAnimation(group, forKey: "move")
```

Most of the code is self explanatory. We set the properties of `CABasicAnimation` -- the duration of the animation, with a ease-in-ease-out effect, moving it to the right by 50pt.

In the example, we change the `keyPath` of "position.x". We could change any of the [properties](https://developer.apple.com/reference/quartzcore/cabasicanimation), such as "opacity", "backgroundColor", "transform.scale.x", etc.


## Make the Layer Remains in Final State

A [controversial](https://gist.github.com/d-ronnqvist/11266321), but simple, way to make the view remain in the final state is:

```swift
move.fillMode = kCAFillModeForwards
move.removedOnCompletion = false
```

This merely make the **presentation** remains in the final state. The actual **model** is not changed. This will affects hit-testing etc.

If the view does not require user interaction, then you could use this _easy_ way.

Otherwise, you have to update the model after the animation eg.

```swift
view.layer.addAnimation(group, forKey: "move")
view.frame = ... // Set the final state
```


## Multiple Animations

You can combine multiple animations into an animation group.

We extend our example with a fade out effect.

```swift
let move = CABasicAnimation()
move.keyPath = "position.x"
move.byValue = 50

let fade = CABasicAnimation()
fade.keyPath = "opacity"
fade.byValue = -1

let group = CAAnimationGroup()
group.animations = [move, fade]
group.duration = 1
group.timingFunction = CAMediaTimingFunction(name: kCAMediaTimingFunctionEaseIn)
group.fillMode = kCAFillModeForwards
group.removedOnCompletion = false

view.layer.addAnimation(group, forKey: "move-fadeout")
```

We use the `CAAnimationGroup` and set the common properties to the group instead.


## Animate key frames

We can also animate key frames using `CAKeyframeAnimation`, by changing the values with precise timing:

```swift
let move = CAKeyframeAnimation()
move.keyPath = "position.x"
move.values = [0, 10, 50]
move.keyTimes = [0, 0.1, 0.2]
...
```


## On Animation Completion

```swift
CATransaction.begin()
        
CATransaction.setCompletionBlock {
    // Perform this when animation has completed
    ...
}

let animation = ...
view.layer.addAnimation(animation, forKey: "animation")

CATransaction.commit()        
```


## Pitfall with Opacity and Interaction

There is a pitfall when you animate a view's opacity, yet wants to enable interaction.

When you set a view `opacity` to 0 (hide it), iOS will actually implicity disable touches/interaction (though `userInteractionEnabled` will still be true).

Then in a fade in animation, you increase the opacity back to 1, and expect the view to respond to touch.

BUT, remember: the animation code only affects the presentation, not the actual model. Read the section above on "Make View Remains in Final State".

The solution is to explicitly set opacity back to 1 after the animation ended.

```swift
CATransaction.setCompletionBlock {
    self.view.layer.opacity = 1
}
```


## New: UIViewPropertyAnimator for iOS 10

A new feature for **iOS 10**. 

An add-on to Core Animation, providing the power to move through the animation's progress however we ant to. Well explained by [Arek Holko](http://holko.pl/2016/07/07/popping-into-uiviewpropertyanimator/).

So if you want to controll the animation progress (reverse on cancel?), then [learn the ropes](https://www.shinobicontrols.com/blog/ios-10-day-by-day-day-4-uiviewpropertyanimator).
