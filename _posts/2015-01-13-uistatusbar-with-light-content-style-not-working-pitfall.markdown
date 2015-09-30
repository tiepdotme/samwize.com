---
layout: post
title: "UIStatusBar with light content style not working - pitfall"
date: 2015-01-13 10:42:40 +0800
comments: true
categories: [iOS]
---

This is a common pitfall when you are using [UIStatusBarStyleLightContent](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/TransitionGuide/Bars.html) for your status bar when you want light colored status text over dark colored background.

There are 2 things that you MUST set to make it work.

1. This is usually what developers will do - Under Target settings > General > Status Bar Style > Change to Light. This will effect the Info.plist to include UIStatusBarStyleLightContent.

2. This is an obscure/unnatural step - In Info.plist, add `View controller-based status bar appearance` and set to `NO`

