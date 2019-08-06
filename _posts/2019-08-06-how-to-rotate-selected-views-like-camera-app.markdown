---
layout: post
title: "How to Rotate Only Selected Views (like Camera app)"
date: 2019-08-06T17:43:46+08:00
categories: [iOS]
---

Camera app has an interesting behavior -- When you rotate from portrait to landscape, only some buttons will rotate, while the camera preview layer and texts do not.

This post is a guide on creating such UI behavior.

## 1. Lock to only Portrait Orientation

In your `Info.plist`, edit `UISupportedInterfaceOrientations` to have only `UIInterfaceOrientationPortrait`.

We don't want the other orientations, because we will manually handle them.

## 2. Observe `orientationDidChangeNotification`

In your view controller, observe for the event when device orientation change.

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    UIDevice.current.beginGeneratingDeviceOrientationNotifications()
    NotificationCenter.default.addObserver(self, selector: #selector(deviceOrientationDidChange),
        name: UIDevice.orientationDidChangeNotification, object: nil)
}
```

## 3. Handle the rotation event

This is where the magic happens. A very simple magic, by setting the `transform` property of each view.

```swift
@objc private func deviceOrientationDidChange() {
    let viewsToRotate = [button1, button2, labelXX, viewXX]
    let rotation = UIDevice.current.orientation.rotationTransform
    viewsToRotate.forEach {
        $0.transform = rotation
    }
}
```

`rotationTransform` is a `UIDeviceOrientation` extension that provide us the **required rotation** according to the device orientation, as follows:

```swift
extension UIDeviceOrientation {
    var rotationTransform: CGAffineTransform {
        switch self {
        case .landscapeLeft: return CGAffineTransform(rotationAngle: .pi/2)
        case .landscapeRight: return CGAffineTransform(rotationAngle: -.pi/2)
        case .portraitUpsideDown: return CGAffineTransform(rotationAngle: .pi)
        default: return .identity
        }
    }
}
```

Of course, to polish with an animation, you can wrap with `UIView.animate()`.
