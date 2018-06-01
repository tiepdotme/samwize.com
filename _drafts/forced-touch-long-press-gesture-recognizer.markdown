---
layout: post
title: "Forced Touch + Long Press Gesture Recognizer"
date: 2018-05-15T13:56:15+08:00
categories: [iOS]
published: false
---


## [UILongPressGestureRecognizer](https://developer.apple.com/documentation/uikit/uilongpressgesturerecognizer)

There is a concrete gesture subclass to use, [defaulting](https://developer.apple.com/documentation/uikit/uilongpressgesturerecognizer/1616423-minimumpressduration) "long press" to 0.5 seconds.

> Long-press gestures are [continuous](https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/handling_uikit_gestures/handling_long_press_gestures)

What is **continuous**?

It means the "action" or "handler" is called continuously, multiple times when the gesture begins, changed, and ended.

https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/handling_uikit_gestures/handling_long_press_gestures

## Forced Touch

aka 3D Touch or Deep Press

https://github.com/FlexMonkey/DeepPressGestureRecognizer

## Sensitive Library

https://github.com/igormatyushkin014/Sensitive

A nice helper using closures.

https://krakendev.io/force-touch-recognizers/
Normalize the force, nothing much else to learn, but SEO good.
