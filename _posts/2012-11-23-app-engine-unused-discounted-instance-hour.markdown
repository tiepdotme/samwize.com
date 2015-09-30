---
layout: post
title: "App Engine Unused Discounted Instance Hour"
date: 2012-11-23 22:48
comments: true
categories: [AppEngine, Cloud]
---

Google App Engine has this item - Discounted Instance Hour - which you can reserve a number of hours per week at a cheaper cost.

Recently, I was looking at the Charge Sent per week, and one of the item is this:

> Unused Discounted Instance Hour - Used XXX hours

<!-- more -->

The item cost a big part of my weekly charges. I was puzzled at first: How did I used XXX hours out of an item that said is unused?

Then I went to my Billing Settings, and saw this:

	Discounted Instance Hours: YYY Hours

	Committing to a number of instance hours for a weekly billing period in advance can 
	help lower your bill. You will be charged $0.05 per hour for the hours you commit to here, 
	even if you don't use them during the billing period.

I have set YYY hours per week as a **Reserved** Discounted Instance Hours.

Even if I don't use them, they are charged!

## Lesson Learnt ##

At times, you should monitor the weekly usage.

If the **Unused Discounted Instance Hour** is very high (much higher than what is being used), then you should adjust it from Billing Settings. 

I have wasted quite some money paying for instance hours that I didn't use..

(I set the reserved instance hours very high initially because there was a period of peak, but now it has gone down, yet I didn't adjust..)