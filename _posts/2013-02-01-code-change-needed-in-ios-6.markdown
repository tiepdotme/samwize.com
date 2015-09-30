---
layout: post
title: "Code Change Needed in iOS 6"
date: 2013-02-01 00:03
comments: true
categories: iOS
published: true
---

Some tips on your iOS code change needed for supporting iOS 6.

<!-- more -->

## presentModalViewController ##

Starting from iOS 6, `UIViewController` deprecated this:

	- (void)presentModalViewController:(UIViewController *)modalViewController animated:(BOOL)animated

Starting from iOS 5, the replacement is:

	- (void)presentViewController:(UIViewController *)viewControllerToPresent animated: (BOOL)flag completion:(void (^)(void))completion

If you *really* need to support the older iOS 4 and below, and mix in the new replacement, you would need to [check](http://stackoverflow.com/questions/12445190/dismissmodalviewcontrolleranimated-deprecated) using `respondsToSelector`.

    if ([self respondsToSelector:@selector(presentViewController:animated:completion:)])
        [self presentViewController:navController animated:YES completion:nil];
    else
        [self presentModalViewController:navController animated:YES];





## For older xib ##

### Tab Bar not working ###

If after adding support for iOS6 and your UITabBar is not working anymore eg. does not respond to taps, it could be due to the xib from an older project.

You [need](http://stackoverflow.com/a/12627117/242682) to set the `window` object of your main xib to **Full Size at Launch**.



### Error: UIViewControllerHierarchyInconsistency ###

This is again due to an older project. 

You [need](http://stackoverflow.com/questions/12434937/uiviewcontrollerhierarchyinconsistency-when-trying-to-present-a-modal-view-contr/12450770#12450770) to do this:

1. Move main **View** out of **View Controller**:

2. Delete **View Controller** from the XIB (it is not necessary since File's Owner should be of its Class already)

