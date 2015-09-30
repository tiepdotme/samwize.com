---
layout: post
title: "Camera Programming for iOS 7"
date: 2014-02-11 00:51:30 +0800
comments: true
categories: iOS
---

Apple's [Camera Programming Guide](https://developer.apple.com/library/ios/documentation/AudioVideo/Conceptual/CameraAndPhotoLib_TopicsForIOS/Articles/TakingPicturesAndMovies.html#//apple_ref/doc/uid/TP40010406-SW8) for taking pictures and movies is outdated.

It was last updated on 2012-07-17 (as of writing), and this doesn't take into account of new stuff eg. iOS 7 changes and ARC

<!-- more -->

In iOS 5, the behaviour of UIViewController `parentViewController` has [changed](http://stackoverflow.com/a/8349487/242682).

> Prior to iOS 5.0, if a view did not have a parent view controller and was being presented, the presenting view controller would be returned. On iOS 5, this behavior no longer occurs. Instead, use the **presentingViewController** property to access the presenting view controller.

Which means when you dismiss the picker, the `parentViewController` could be nil, and hence it will not dismiss as you expected.

This is my updated source code for responding to the user tapping Cancel.

```objc
- (void) imagePickerControllerDidCancel: (UIImagePickerController *) picker {
    UIViewController *dismissingController = nil;
    if ([picker respondsToSelector:@selector(presentingViewController)])
        dismissingController = picker.presentingViewController;
    else
        dismissingController = picker.parentViewController;

    // Since dismissModalViewControllerAnimated is deprecated in iOS 6..
    if ([dismissingController respondsToSelector:@selector(dismissViewControllerAnimated:completion:)]){
        [dismissingController dismissViewControllerAnimated:YES completion:nil];
    } else {
        [dismissingController dismissModalViewControllerAnimated:YES];
    }
}
```

I also dropped `[picker release]` since using ARC.

Similarly, replace the code for dismissing when a photo/video is selected.
