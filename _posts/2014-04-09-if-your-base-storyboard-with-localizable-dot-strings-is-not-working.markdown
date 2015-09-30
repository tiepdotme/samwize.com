---
layout: post
title: "If your base storyboard with localizable.strings is not working"
date: 2014-04-09 11:49:15 +0800
comments: true
categories: [iOS, Localization]
---

I have everything setup correctly, but just somehow the localized string is not correctly showing on simulator/device. It is still showing the base string.

I find out this could be due to a change in your base storyboard, and yet it is not correctly reflected.

To resolve:

- Select the localized storyboard eg. Chinese (Simplified)

- In File Inspector, toggle from "Localizable Strings" to "Interface Builder Cocoa Touch Storyboard". This will retain the strings you already had, so you don't have to worry.

- Now, change it back to "Localizable Strings", and things should be updated!
