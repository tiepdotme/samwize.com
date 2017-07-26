---
layout: post
title: "Nested UIViewControllers Using Container"
date: 2017-07-10T16:33:12+08:00
categories: []
published: false
---

https://developer.apple.com/library/content/featuredarticles/ViewControllerPGforiPhoneOS/ImplementingaContainerViewController.html

https://stackoverflow.com/questions/17499391/ios-nested-view-controllers-view-inside-uiviewcontrollers-view
https://stackoverflow.com/questions/30219631/is-it-bad-practice-to-put-uiviewcontrollers-in-other-uiviewcontrollers - go answer

http://sandmoose.com/post/35714028270/storyboards-with-custom-container-view-controllers
Storyboard

-(void)viewDidLoad{
     [super viewDidLoad];
     InnerViewController *innerViewController = [self.storyboard instantiateViewControllerWithIdentifier:INNER_VIEW_CONTROLLER];
     [self addChildViewController:innerViewController];
     [self.view addSubview:innerViewController.view];
     [innerViewController didMoveToParentViewController:self];
}

https://spin.atomicobject.com/2015/10/13/switching-child-view-controllers-ios-auto-layout/

https://github.com/codepath/ios_guides/wiki/Container-View-Controllers-Quickstart

Vs just nest the VC and add the view