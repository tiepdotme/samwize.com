---
layout: post
title: "Why an Android app is not supported for a particular device?"
date: 2013-05-07 22:46
comments: true
categories: [Android]
---

This is what happend to my Android app.

It works well when I install and test on my Lenovo IdeaPad Tablet (it could be any other device). 

But after I upload the APK to Google Play Store, I could not find the app using the tablet!

<!-- more -->

After researching, I realized it is to do with the features and permissions declared in the manifest file. Google Play has [filtering](http://developer.android.com/google/play/filters.html).

The documentation that you must read is on the [uses-feature element](http://developer.android.com/guide/topics/manifest/uses-feature-element.html#market-feature-filtering). 

Key takeaway:

- If you don't use a feature, you must EXPLICITLY say that `required=false`!

- [Permissions imply Features](http://developer.android.com/guide/topics/manifest/uses-feature-element.html#permissions)

Android has grown complex.