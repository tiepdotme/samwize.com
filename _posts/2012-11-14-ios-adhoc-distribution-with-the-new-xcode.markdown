---
layout: post
title: "iOS Adhoc Distribution with the new Xcode"
date: 2012-11-14 22:00
comments: true
categories: iOS
---

If you are an iOS developer, you are probably familiar with the 3 ways to install an app - Run/debug on device, Ad hoc distribution, and App Store distribution.

For each of the 3 ways, Apple provisioning portal has provided the provisioning certs you must use to sign the binary.

**Adhoc distribution** is the most complicated (the other 2 ways are very straight forward). Over the years, the process has also changed.

This post is for development with Xcode 4.5.

<!-- more -->

## In the past ##

In the early days (year 2008), the guide to Adhoc distribution was to create a new build configuration called "Adhoc", in addition to "Debug" and "Release". 

I know why it is necessary back then, but that is not necessary now. But some posts still [insisted](http://diaryofacodemonkey.ruprect.com/2011/03/18/ad-hoc-app-distribution-with-xcode-4/) that.


## The new way ##

With Xcode 4.5 (or perhaps earlier), Project Schemes is introduced. It provides more ways you can configure how you "Build", "Run", "Test", "Analyse" and "Archive". You can change the build configuration to "Debug" or "Release" as you wanted.

You don't need to touch them.

To distribute Adhoc way, you only need to "Archive".

Then in Organizer, click on the "Distribute" button, and select "Save for Enterprise or Ad-Hoc Deployment".

Select the Adhoc provisioning cert (you would need to download from Apple portal, and install to Xcode).

{%img /images/xcode-distribute-adhoc-enterprise.png Adhoc Distribution %}

Save the `App.ipa`.

To install, drag to iTunes and sync. That's it!


## Push Notification ##

Things get slightly more complicated if you are using Apple Push Notifications. 

To use push, there are 2 more certs needed for your server to communicate with Apple. One is `aps-development`, the other is `aps-production`.

You use `aps-development` for running on device using "Debug" build configuration.

You use `aps-production` for both Adhoc and App Store distribution.

Not the other way round. Adhoc distributed apps will NOT get push when you use `aps-development`.

Why? 

> Because Adhoc distributed apps will register device token to **Apple production gateway**, while your `aps-development` communicates with **Apple sandbox gateway**.

If you want Adhoc distributed apps to get push, you must send the push with `aps-production`. Adhoc is as good as production app, yet not on the App Store.


## Conclusion ##

In the past, you need to create a new build configuration call "Adhoc". But that is no longer needed as you can select which cert to re-sign after archiving.

I will not cover wireless distribution today (though you may refer to [this method using dropbox](http://blog.just2us.com/2010/12/wireless-ad-hoc-distribution-for-iphone-apps/)).

Or you could use [TestFlight](https://testflightapp.com/) which is the best in distributing ad hoc builds - with versioning and beta users management!
