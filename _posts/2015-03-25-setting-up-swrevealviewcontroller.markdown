---
layout: post
title: "Setting up SWRevealViewController"
date: 2015-03-25 23:27:15 +0800
comments: true
categories: [iOS]
---

[SWRevealViewController](https://github.com/John-Lluch/SWRevealViewController) is a very well written iOS library by John Lluch.

It is a UI component popularly known as the hamburger menu that slides in the sidebar. 

This post is all about my encounter with SWRevealViewController.

<!-- more -->

## Using Hamburger Menu

I was once very against the use of the Hamburger Menu.

The hamburger is commonly placed on the top left corner, and that is where the thumb can't reach on an iPhone.

But overtime, I felt the UI pattern can be used, if a swipe right gesture is provided.

One of the most well implemented app is Slack.

You can easily swipe right to show the channels, or swipe left to open a side menu.


## How to setup SWRevealViewController

The [github](https://github.com/John-Lluch/SWRevealViewController) has some information on using the API, and a sample app.

But there ain't good steps.

If you are integrating to your project, this is my step-by-step guide.

1. Add `pod 'SWRevealViewController', '~> 2.3'` to your [pods](http://cocoapods.org)

2. Create a View Controller (VC) in your storyboard, and set the class to `SWRevealViewController`. This VC will be empty. 

3. Create a `Front VC` and a `Rear VC` in your storyboard. 

    A "Front VC" is usually the content view controller, and you usually will have multiple of them, with one as the initial one. 

    A "Rear VC" is usually a sidebar on the left of the "Front VC" when slided in.

    There is also a "Right VC", which is sort of a "Rear VC" but on the right. We will omit this, but every API for Rear has one for Right.

4. Create a segue from `SWRevealViewController` to `Front VC`, when prompted set to “reveal view controller set segue”, then set the segue identifier to `sw_front`.

5. Create a segue from `SWRevealViewController` to `Rear VC`, when prompted set to “reveal view controller set segue”, then set the segue identifier to `sw_rear`.

6. Add a button to Front VC with the name `sidebarButton`

7. The code for Front VC:

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    SWRevealViewController *revealViewController = self.revealViewController;
    if (revealViewController) {
        [self.sidebarButton setTarget: self.revealViewController];
        [self.sidebarButton setAction: @selector(revealToggle:)];
        [self.view addGestureRecognizer: self.revealViewController.panGestureRecognizer];
    }
}
```

That's all! You can try running and tap on the button to reveal the side bar, or swipe!

Add the code similarly for Rear VC, for the menu item to reveal back to (another) Front VC.


## More customization explained

```objc
    // Example sidebar is width 200
    revealViewController.rearViewRevealWidth = 200;
    
    // Cannot drag and see beyond width 200
    revealViewController.rearViewRevealOverdraw = 0;    
    
    // Faster slide animation
    revealViewController.toggleAnimationDuration = 0.2;
    
    // Simply ease out. No Spring animation.
    revealViewController.toggleAnimationType = SWRevealToggleAnimationTypeEaseOut;
    
    // More shadow
    revealViewController.frontViewShadowRadius = 5;
```

There are more, but above is what I find useful for customizing my UI. 

Check out `SWRevealViewController.h` for more.




