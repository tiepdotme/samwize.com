---
layout: post
title: "Always Needed Guide to Customize UINavigationBar"
date: 2015-04-11 00:12:41 +0800
comments: true
categories: [iOS]
---

Customizing `UINavigationBar` remains a _not-easy-to-remember_ topic.

When I need to change the looks of the nav bar - such as changing the Back arrow, or title, or making it transparent - I always need to google for the code.

<!-- more -->

## Understanding Navigation Bar

The reason that nav bar is not easy, is because it is a pretty complicated component.

There are 3 classes associated with nav bar:

1. [UINavigationBar](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UINavigationBar_Class/index.html)
2. [UINavigationItem](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UINavigationItem_Class/index.html)
3. [UIBarButtonItem](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIBarButtonItem_Class/index.html)

`UIBarButtonItem` are the buttons on the `UINavigationBar`.

But it is actually the `UINavigationItem` that you will use to manage the buttons and views. 

Specifically, eg you are on a view controller, then `self.navigationItem` will provide you the left button, right button, title, etc.

One of the most confusing thing is between `self.navigationItem.backBarButtonItem` and `self.navigationItem.leftBarButtonItem`.

Assuming you are now on your view controller, then `leftBarButtonItem` refers to the button on the left of the nav bar. `backBarButtonItem` is NOT shown (yet). 

`backBarButtonItem` will be shown when you push another view controller (on top of it), as the left button of this topmost view controller (the "Back").


## Change the Back Arrow Image

You can change the default back arrow image by setting the appearance proxy.

    [[UINavigationBar appearance] setBackIndicatorImage:[UIImage imageNamed:@"back-button-image"]];
    [[UINavigationBar appearance] setBackIndicatorTransitionMaskImage:[UIImage imageNamed:@"back-button-image"]];


## Change the Back Title

The default "Back Title" is the title of the view controller. 

Sometimes, you want it to display literally "Back" (a shorter word if your title is too long), or you might want to not display a string at all (just the Back Arrow).

    self.navigationItem.backBarButtonItem = [[UIBarButtonItem alloc] initWithTitle:@"Back" style:UIBarButtonItemStylePlain target:nil action:nil];

Remember: The above has to be set whereby `self` is the parent view controller which the button will go back to.


## Make the Background Transparent

For this, I have created [UINavigationBar+Addition](https://github.com/samwize/UINavigationBar-Addition).

There are quite a number of things to set, some are not obvious. 

    [navBar setTranslucent:YES];
    [navBar setBackgroundImage:[[UIImage alloc] init] forBarMetrics:UIBarMetricsDefault];
    navBar.backgroundColor = [UIColor clearColor];
    navBar.shadowImage = [UIImage new];
