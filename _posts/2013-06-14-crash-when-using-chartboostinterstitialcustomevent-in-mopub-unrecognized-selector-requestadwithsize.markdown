---
layout: post
title: "Crash when using ChartboostInterstitialCustomEvent in MoPub: unrecognized selector requestAdWithSize"
date: 2013-06-14 00:50
comments: true
categories: [iOS]
---

While integrating [Chartboost](https://chartboost.com/) into [MoPub](http://mopub.com/) for an iOS app, I encountered a crash every time there is a Chartboost ad loading.

The error is:

	ChartboostInterstitialCustomEvent requestAdWithSize customEventInfo unrecognized selector 

<!-- more -->

I thought it is a bug with MoPub at first (my experience tells me as MMedia crashes at times too). 

After researching a while, I found out the crash is because MoPub tried to load a banner ad, while Chartboost does not have banner ads! Chartboost ONLY has interstitial ads!

To solve, you have to disable banner ads in MoPub portal for Chartboost..