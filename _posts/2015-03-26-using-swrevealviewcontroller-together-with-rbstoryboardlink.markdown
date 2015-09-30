---
layout: post
title: "Using SWRevealViewController together with RBStoryboardLink"
date: 2015-03-26 11:08:32 +0800
comments: true
categories: [iOS]
---

I posted about [how to modularize storyboard using RBStoryboardLink](/2015/02/27/how-to-break-into-multiple-storyboards-in-a-project/) and [how to use SWRevealViewController](/2015/03/25/setting-up-swrevealviewcontroller/).

But to use both of them together is not straight forward.

This is because both library makes use of custom segues to do what they want to do. And it is not possible for you to customize 1 segue to satisfy both!

<!-- more -->

To make them work together:

1. Create the `RBStoryboardLink` suggorate view controller and setup `storyboardName` as per normal, but don't create any segue to it. It will be alone.

2. Give it a storyboard ID eg. "surrogate"

3. Don't use `SWRevealViewControllerSeguePushController` segue too

4. Instead, call the revealViewController's `pushFrontViewController` method manually. 

The code:

```objc
- (IBAction)buttonTapped:(id)sender {
    UIViewController *vc = [self.storyboard instantiateViewControllerWithIdentifier:@"surrogate"]; 
    [self.revealViewController pushFrontViewController:vc animated:YES];
}
```

If the view controller is not getting behind the status bar correctly, set `needsTopLayoutGuide` key to NO in the suggorate.
