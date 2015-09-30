---
layout: post
title: "Guide to using Unwind Segues"
date: 2015-04-23 09:46:55 +0800
comments: true
categories: [iOS]
---

Unwind segue is a very nice concept in iOS to "go back" to a previous view controller.

There is an [Apple Guide](https://developer.apple.com/library/ios/technotes/tn2298/_index.html) to using it, and a good [in a nutshell](http://stackoverflow.com/a/15839298/242682) about it.

Here, I will share how I use it in real world applications.

<!-- more -->

## Basics and Pitfalls

- You create this method in the view controller which let others unwind to it:

    `- (IBAction)unwindToHere:(UIStoryboardSegue*)sender {...}`

- The view controller MUST be present in the navigation hierarchy. Obvious perhaps, but good to note unwind is to "go back", not jump to anywhere.
- The view controller should have appeared before performing other segues, otherwise you will encounter [Warning: Attempt to present on whose view is not in the window hierarchy!](http://stackoverflow.com/q/20250481/242682)


## How I use unwind

I always have a `FirstViewController` which is the initial view controller on my storyboard.

This `FirstViewController` is like a coordinator to other view controllers such as Login, Main, etc.. 

The advantage of having that is since it is the initial view controller, it will always be in the navigation hierarchy. And thus, it is a good candidate to unwind to (will always succeed!).

```objc
@interface FirstViewController ()
// Use this to coordinate unwind
@property (nonatomic, strong) NSString *segueIdentifierToUnwindTo;
@end

@implementation FirstViewController
- (void)viewDidAppear:(BOOL)animated {
    [super viewDidAppear:animated];   

    // Handle unwind
    if (_segueIdentifierToUnwindTo) {
        [self performSegueWithIdentifier:_segueIdentifierToUnwindTo sender:self];
        self.segueIdentifierToUnwindTo = nil;   // reset
        return;
    }
    
    // Logics to segue 
    // ...
}

// Example of an unwind segue "gotoLogin"
- (IBAction)gotoLogin:(UIStoryboardSegue*)sender {
    // Don't perform segue directly here, because in unwind, this view is not in window hierarchy yet!
    // [self performSegueWithIdentifier:@"Login" sender:self];
    self.segueIdentifierToUnwindTo = @"Login";
}

// Example of another unwind segue "gotoMain"
- (IBAction)gotoMain:(UIStoryboardSegue*)sender {
    self.segueIdentifierToUnwindTo = @"Main";
}

@end
```

In the example above, I have 2 unwind segues `gotoLogin` and `gotoMain`. You could have as many unwind segues.

When you add an unwind segue to a scene (read [here](https://developer.apple.com/library/ios/technotes/tn2298/_index.html) on how), you could choose `gotoLogin`. That is bascially asking to unwind to `FirstViewController`, and then asking `FirstViewController` to go to Login next.

If it is an unwind operation, the segue is performed right after `viewDidAppear`. 

If it is not an unwind operation, other logics could be added in `viewDidAppear` eg. go to Login if no logged in user.

