---
layout: post
title: "Internal error. Cannot find an outgoing row head for incoming head"
date: 2014-04-15 09:47:23 +0800
comments: true
categories: [iOS]
---

It happened when I was working on an old project that use nib, and I converted it to use autolayout.

Everything was fine, except when I tried to present a table view controller, it crashed.

> internal error. Cannot find an outgoing row head for incoming head

This is an error which I could not find a proper solution. But here's what I did.

<!-- more -->

# 1. Convert to storyboard

I suspect it got to do with the nib, and therefore I [converted](https://developer.apple.com/library/ios/releasenotes/Miscellaneous/RN-AdoptingStoryboards/) them to Storyboard.

It isn't that hard. In fact, in 5 minutes, I am done for my main window.

You can copy and paste the views and constraints, so that save a lot of time.

All you left to do is to select the correct View Controller class, then link up to the views and actions.

And that solve the internal error! But.. it still crash when run in iPad.


# 2. Popover in iPad.

When I present the table view, I use a modal segue.

Somehow, that cannot work for iPad.

The solution is to change to popover segue. (I use a separate storyboard for iPad)

I am not sure if this is an Xcode bug, but that solve the irritating problem.
