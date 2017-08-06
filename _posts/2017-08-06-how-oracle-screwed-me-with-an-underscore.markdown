---
layout: post
title: "How Oracle screwed me for 2 months with an UNDERSCORE"
image: /images/pushio-screw-me.jpg
date: 2017-08-06T00:25:18+08:00
categories: [Rant, API]
---

_tldr; Don't use an enterprise product. In this case Oracle's Responsys/PushIO sucks badly._

This post is my rant on how bad an enterprise product from Oracle is.

## The ridiculous bug

I spent 2 months to figure out why our iOS app did not display rich message pushed from Oracle's platform. It boils down to an UNDERSCORE.

Yes, freaking `_`, generated in their API key is the reason why our app didn't work.

![](/images/pushio-ridiculous-bug.jpg){:.border}

Quoting reply from developer support:

> The bug is that your API key was generated with an underscore "_" and that is an invalid character for how the API key is used downstream.

> The fix should be pretty straighforward... create a new platform until you get one without underscores in the API key.

Wow.

It is an embarassing bug, but what appal me more is how they deal with it.

## A major bug

Firstly, the bug affects majority of their users, since it took me 3 tries, before getting a "valid" one.

I estimate **66% of users** could have generated "invalid" keys.

## Not fixing their SDK quickly

[PushIO last release](https://github.com/pushio/PushIOManager_iOS/tags) of their SDK is 16 Jun 2017, that is 2 months ago as of writing.

_I give them the benefit of doubt, as I assume the bug only appear in that release, though I do believe the bug happens way before version 6.32.1._

The point is, a major bug should be fixed quickly, yet it wasn't.

Not to mention this should be a easy fix. They could fix their API key generator probably in a few lines of code. Somehow they did not.

## Lack of knowledge on the issue

Our account manager and a couple of customer support officers did not know about the `_` issue, until it escalates to their developer.

It could mean:

1. They don't have many users and this issue never came up
2. They have a problem with communicating and sharing of such knowledge

Either way, as a user, I am less confident with Oracle.

## We took 2 long months

Why so long?

While trying to investigate the issue and isolate the problem, I asked a simple quesion: _**Is there an iOS sample code?**_

It took 2 weeks, before they finally have an answer -- no, they don't. Again, it reveals a lack of knowledgebase to such FAQ.

Analyzing why a push fails is tedious and slow, because I have to **wait till the next day** to check the logs from an **FTP server**. It feels so 1990s.

Reply from "enterprise support" isn't lightning fast either. _I admit I don't reply them fast too, but I have much more meaningful features to develop._

## A problem with the enterprise suite of products

Responsys is an enterprise email marketing tool -- a slow and complicated web app. [Mail Chimp](https://mailchimp.com) will be it's rival that is cool and user friendly.

PushIO is a mobile push SDK, in a very crowded space since 2009 when Apple revolutionized push. Cool rivals include [Urban Airship](https://www.urbanairship.com) and [OneSignal](https://onesignal.com/).

In 2014, Oracle _eat_ Responsys, who in turn _eat_ PushIO -- a [**$1.5 billion acquisition**](https://www.bizjournals.com/denver/blog/boosters_bits/2014/01/repsonsys-buys-push-io-before-being.html)!

When a large corporation acquires a team and it's product, it is the **start of a downfall**. This case will be messier since a fat clumpsy fish eat a fish which eat another small fish.

The small fish (PushIO) probably died tragically.

![](/images/pushio-big-fish-eat-little-fish.jpg)
