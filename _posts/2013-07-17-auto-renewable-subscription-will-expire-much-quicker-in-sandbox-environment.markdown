---
layout: post
title: "Auto-renewable Subscription will expire much quicker in sandbox environment"
date: 2013-07-17 01:54
comments: true
categories: [iOS]
---

One mistake I had when implementing Auto-renewable Subscription (a kind of In-app purchase) is not understanding how the sandbox environment works.

That resulted in wasted hours in debugging..

You should know [this](http://developer.apple.com/library/ios/#documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/13_ManagingIn-AppPurchases/ManagingIn-AppPurchases.html#//apple_ref/doc/uid/TP40011225-CH4-SW1):

<!-- more -->

Time flies much faster in sandbox than actual. So a 1 week subscription will be only 3 minutes in sandbox. This actually helps because you can test your app much faster.

|Actual duration	| Sandbox duration		|
|-------------------|-----------------------|
|1 week				|3 minutes
|1 months			|5 minutes
|2 months			|10 minutes
|3 months			|15 minutes
|6 months			|30 minutes
|1 year				|1 hour

Also, it will automatically renew **maximum 6 times**.

_UPDATE: WWDC 2018 seems to change to maximum 5 renewals_

Here's a screenshot right from WWDC 2018:

![Condensed intervals for development](/images/subscription-sandbox-interval.png)
