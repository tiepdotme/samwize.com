---
layout: post
title: "WARNING: Do not use Base Internationalization in iOS 5"
date: 2012-11-22 21:57
comments: true
categories: iOS
---

iOS 5 does NOT support Base Internationalization.

I learnt this the hard way (and partly Apple's fault).

After upgrading my project to use Base Internationalization, I release a new version, and shortly, my users complained of startup crash.

<!-- more -->

## New release, crash on startup ##

You might be wondering:

> Didn't you test your app?

I did test, **but only with Simulator**. I ran on Simulator with iOS 5, and also tested on my real device with iOS 6.0.

It didn't crash on Simulator with iOS 5.

However, on a real iPhone with iOS 5, it crashed on startup.

Exception is raised from `[UINib instantiateWithOwner:options:]`, with the error log:

	Terminating app due to uncaught exception 'NSInternalInconsistencyException', 
	reason: 'Could not load NIB in bundle: 'NSBundle 
	</var/mobile/Applications/***/***.app> (loaded)' with name 'MainWindow''

This [meant](http://stackoverflow.com/questions/9075298/app-works-on-simulator-but-not-on-iphone) that the app could not find `MainWindow.xib`. 

It is strange that the xib is not copied, especially when I am prudent to do clean build and run.


## The culprit ##

Base Internationalization was introduced in iOS 6. It is an awesome feature because: instead of having N NIB files for N languages, you need just **1 base NIB file** with **N localization.strings**. 

The biggest change I had for the release was Base Internationalization.

Researching into the problem, I realized Base Internationalization is supported only from iOS 6.

Xcode will let you release the app with *any minimum deployment target*, without any warning. But once the app starts and load a xib, it will crash as it can't find the xib.


## The Solution ##

The change is reversible.

Select the XIB and look under File Inspector.

You can change each of the `localization.strings` back to XIB. Then remove the Base (when prompted, copied it to English).


## Lessons Learnt ##

- Simulator is not exactly like real device

- Base Internationalization is awesome, but is not advised to be used 

- Many users stick with iOS 5 because of Map app. Perhaps they will upgrade in ~2014 as Apple continues to improve.
