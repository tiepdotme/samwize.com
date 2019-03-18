---
layout: page
title: "Core UI Libraries"
permalink: /libs/core-ui/
---

## Animate views during transition

[Hero](https://github.com/lkzhao/Hero) easily animate a view to another view during a transition. It is like Keynote's Magic Move, for iOS.

## Empty State for Table/Collection

[DZNEmptyDataSet](https://github.com/dzenbot/DZNEmptyDataSet) create a view when your table/collection view is empty. You customize it with attributed strings for the title, subtitle and the call-to-action-button. If the standard layout is too enough, you may return your own view.

## Keyboard & Text Fields

[IQManagerKeyboard](https://github.com/hackiftekhar/IQKeyboardManager) prevents keyboard from obscuring the text field, by automatically adjusting the views upwards.

## Animations

[MengTo/Spring](https://github.com/MengTo/Spring) simplify how you write animation code. You may even specify the animation properties in storyboard, without writing any code!

Before Spring, there is [Pop](https://github.com/facebook/pop), a popular animation engine written by Facebook, used in (the now defunct) Facebook Paper app.

## Message/Notification View

[NotificationBanner](https://github.com/Daltron/NotificationBanner) makes it very easy to display messages at the top, kind of like notifications.

[SwiftMessages](https://github.com/SwiftKickMobile/SwiftMessages) is modern, good looking, and very configurable.

[Toast](https://github.com/scalessec/Toast) display message encapsulated in a small popup, for a short duration. Toast is a native UI element borrowed from [Android](https://developer.android.com/guide/topics/ui/notifiers/toasts.html).

## Custom Modal Popup

[Presentr](https://github.com/IcaliaLabs/Presentr) wraps around the iOS 8 custom view controller presentation API for you to easily present popup view.

_Archived [KLCPopup](https://github.com/jmascia/KLCPopup) because Presentr is in Swift!_

## Gestures

Gesture recognizers can be complex, and the old _selector_ way of handling isn't as convenient as using closures. [Sensitive](https://github.com/igormatyushkin014/Sensitive) helps.

## Photo Picker

There are quite a number of projects on selecting media (photo or video), for different use cases, and with different iOS frameworks (UIKit, AVFoundation, Photos, etc).

[ImagePicker](https://github.com/hyperoslo/ImagePicker) is the most starred. It opens the camera immediately, embedding a horizontally scrolling photo library picker (limited current camera roll). It is beautiful, if that UI is what you need.

[ALCameraViewController](https://github.com/AlexLittlejohn/ALCameraViewController) comes in 2nd, with ability to crop.

[FDTake](https://github.com/fulldecent/FDTake) presents an alert sheet first, to choose between photo library or take from camera. Localization is provided (ImagePicker and ALCameraViewController does not come with any localization!). It uses `UIImagePickerController` so all photos in library, moments, camera roll are available.

[QBImagePicker](https://github.com/questbeat/) is a simple picker clone of `UIImagePickerController`, but using either `AssetsLibrary` or the newer `PhotoKit`.
