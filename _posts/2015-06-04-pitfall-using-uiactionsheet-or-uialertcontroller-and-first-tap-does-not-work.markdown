---
layout: post
title: "Pitfall: Using UIActionSheet (or UIAlertController) and first tap does not work"
date: 2015-06-04 11:41:53 +0800
comments: true
categories: [iOS]
---

There is constantly bug with `UIActionSheet`.

Symptoms: Tapping on the action sheet does not work on the first tap. The subsequent tap works as expected.

<!-- more -->

## Since long ago..

Back in 2009, [UIActionSheet has a strange behaviour](http://stackoverflow.com/q/1197746/242682), whereby the tap area seems to have moved.

Solutions such as using `showFromTabBar` or show in window helped to solve Apple's bug.

It wasn't a good experience to have such glaring UI bug in Apple's SDK.


## iOS 8

In iOS 8, Apple introduced `UIAlertController`.

I have once again encountered strange behaviour, whereby I need to tap twice before I can tap a button on the action sheet.

Turned out the issue lies with my `UITextField`, which was the first responder before I present the action sheet.

When I present the action sheet, UIKit will dismiss the keyboard, then present the action sheet. But it didn't do so properly. The first tap will somehow be ignored.

Solution:

```objc
[textfield resignFirstResponder];
[self presentViewController:alert animated:YES completion:nil];
```

Before presenting the action sheet, manually `resignFirstResponder` first.
