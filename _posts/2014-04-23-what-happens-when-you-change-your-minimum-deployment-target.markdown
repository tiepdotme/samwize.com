---
layout: post
title: "What happens when you change your minimum deployment target?"
date: 2014-04-23 15:41:31 +0800
comments: true
categories: [iOS]
---

Things changed in late 2013 with Apple latest ['App Resurrection'](http://techcrunch.com/2013/09/17/apples-app-resurrection-feature-great-for-customers-but-opaque-to-developers/) feature.

For example, if your target was 4.0, then you raised it to 5.0, users still using version 4.x would still be able to install. A prompt like this will be shown:

{%img center http://i.imgur.com/SIGHyKE.png %}

<!-- more -->

This is good news for developer!

We no longer have to worry that we will ignore users with older iOS verions. Less guilt.

I have always been [wary](http://samwize.com/2012/11/22/warning-do-not-use-base-internationalization-in-ios-5/) of increasing my minimum version to iOS 6.0, even though there are tons of new features.

Now, I can just go (latest version - 1), and develop the best for the 90%.

For the rest of 10% of users, they can at least still enjoy the 'resurrected version'.

Nice one Apple.
