---
layout: post
title: "How Oracle screwed me for 2 months with an UNDERSCORE"
image: /images/pushio-screw-me.jpg
date: 2017-08-06T00:25:18+08:00
categories: [Rant, API]
---

_tldr; I was integrating Oracle's mobile push SDK and was stuck for 2 months. Turned out they have an embarassing bug involving an underscore! Oracle's Responsys/PushIO proved right -- enterprise product sucks._

This post is my rant on how bad Oracle's product is.

## The ridiculous bug

I spent 2 months to figure out why our iOS app did not display rich message pushed from Oracle's platform. It boils down to an UNDERSCORE.

Yes, freaking `_`, generated in their API key is the reason why our app didn't work.

![](/images/pushio-ridiculous-bug.jpg){:.border}

From Oracle's developer support:

> The bug is that your API key was generated with an underscore "_" and that is an invalid character for how the API key is used downstream.

> The fix should be pretty straighforward... create a new platform until you get one without underscores in the API key.

Wow.

It is an embarassing bug, but what appal me more is how they deal with it.

## A major bug

Firstly, the bug affects majority of their users, since I took 3 tries, before getting a "valid" one.

I estimate **66% of users** could have generated "invalid" keys.

The bug has big impact. All rich push messages will NOT be displayed in the app!

## Not fixing their SDK quickly

[PushIO last release](https://github.com/pushio/PushIOManager_iOS/tags) of their SDK is 16 Jun 2017, that is 2 months ago as of writing.

_I give them the benefit of doubt, as I assume the bug only appear in that release, though I do believe the bug happens way before version 6.32.1._

The point is, a major bug should be fixed quickly. But Oracle did not.

Not to mention this should be an easy fix. They could fix their API key generator, probably in a few lines of code. Somehow Oracle choose not to.

## Lack of knowledge on the issue

Our account manager and a couple of customer support officers did not know about the embarassing `_` issue, until it escalated to their developer.

It could mean:

1. They don't have many users and this issue never came up
2. They have a problem with communicating and sharing of such knowledge

Either way, as a user, I am less confident with Oracle.

## We took 2 long months

Why so long?

While trying to investigate the issue and isolate the problem, I asked a simple quesion: _**Is there an iOS sample code?**_

Replies from "enterprise support" ain't lightning fast. 

It took 2 weeks, before they finally have an answer -- no, they don't. Again, it reveals a lack of knowledgebase to such FAQ.

Investigating why our push failed is tedious and slow, because I have to **wait till the next day** to check the logs from an **FTP server**. It feels so 1990s.

## A problem with the enterprise suite of products

Responsys is an enterprise email marketing tool -- a slow and complicated web app. [Mail Chimp](https://mailchimp.com) on the other hand, a rival, is cool and user friendly.

PushIO is a mobile push SDK, in a very crowded space since 2009, when Apple revolutionized push. It's better rivals include [Urban Airship](https://www.urbanairship.com) and [OneSignal](https://onesignal.com/) (free!).

In 2014, Oracle _eat_ Responsys, who in turn _eat_ PushIO -- a in [**$1.5 billion acquisition**](https://www.bizjournals.com/denver/blog/boosters_bits/2014/01/repsonsys-buys-push-io-before-being.html)!

When a large corporation acquires a team and it's product, it is the **start of a downfall**. This case is messier since a fat clumpsy fish eat a fish which eat another small fish.

The small fish (PushIO) probably died tragically.

![](/images/pushio-big-fish-eat-little-fish.jpg)
