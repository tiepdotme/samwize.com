---
layout: post
title: "Rejection: App Name Not Good Enough"
date: 2019-02-24T14:20:22+08:00
categories: [macOS, App Review]
---

I have no shortage of rejections coming from Apple App Review team.

Today, I encountered another strange one for [my upcoming app](https://whereismymoney.app).

![](/images/rejection-mymoney-app-name.jpg)

## Guideline 2.3.8 - Performance

> We noticed that your app name to be displayed on the App Store does not sufficiently match the name of the app displayed when installed on macOS.

- iTunes Connect Name: **Where Is My Money**
- App Name when Installed: **My Money**
- App Name in About/Quit Menu: **My Money**

I don't see how a different name will affect performance.

The [guideline](https://developer.apple.com/app-store/review/guidelines/) for section 2.3 is for _Accurate Metadata_, while 2.3.8 actually is for rating:

> Metadata should be appropriate for all audiences, so make sure your app and in-app purchase icons, screenshots, and previews adhere to a 4+ age rating even if your app is rated higher. For example, if your app is a game that includes violence, select images that don’t depict a gruesome death or a gun pointed at a specific character. Use of terms like “For Kids” and “For Children” in app metadata is reserved for the Kids Category. Remember to ensure your metadata, including app name and icons (small, large, Apple Watch app, alternate icons, etc.), are similar to avoid creating confusion.

App review is often subjective, as Apple reviewers' guideline is to make sure the names _sufficiently match_.

## Why I didn't use the same name?

The reason that I did not use **My Money** for the iTunes Connect Name (aka the App Store name) is because the name is unavailable.

App Store name is on a first come first serve basis (or unless you have the trademark).

I have a [Torchlight](https://just2us.com/torchlight/) app, and there are tons of competitors, and they keep coming, until Apple banned further Torchlight app from developers..

But many developers are willing to add suffices to App Store name, because it has a big effect on search - App Store Optimization (ASO). Eg. "Torchlight - Brightest LED for iPhone & iPad"

But few years ago, App Store added a **slogan field**, and subsequently limit App Store name to only 30 char.

## What is App Name

App Name is different from App Store name.

It is explained in [Technical Q&A QA1892](https://developer.apple.com/library/archive/qa/qa1892/_index.html). The name comes from `CFBundleDisplayName`, for both app and extensions.

The name is used to display the grid of apps on iOS device.

For macOS, the name is also used in the standard About pane and menu.
