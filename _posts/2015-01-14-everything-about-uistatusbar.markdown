---
layout: post
title: "Everything about UIStatusBar"
date: 2015-01-14 09:45:35 +0800
comments: true
categories: iOS
---

Previously we mentioned a [common pitfall with using a light style status bar](http://samwize.com/2015/01/13/uistatusbar-with-light-content-style-not-working-pitfall/).

This is more to understanding how to properly use `UIStatusBar`.

<!-- more -->

# Info.plist

You already know the 2 settings in the project/target setting, or in Info.plist. There's one more (see 3rd point).

1. `UIStatusBarStyle` - Default is dark text on white. Light is opposite.

2. `UIViewControllerBasedStatusBarAppearance` - Set to NO if you have a global status bar styling

3. `UIStatusBarHidden` - During launch, if you don't want status bar to overlap your launch image, then set to YES.


# Application Launch

In `application:didFinishLaunchingWithOptions:`, you could need 2 more codes to make status bar work.

```objc
// Set the status bar style
[application setStatusBarStyle:UIStatusBarStyleLightContent];

// And you need to un-hide it, if you hide it during launch
[application setStatusBarHidden:NO withAnimation:UIStatusBarAnimationFade];
```

