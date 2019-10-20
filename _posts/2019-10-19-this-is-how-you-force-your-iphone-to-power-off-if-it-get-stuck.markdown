---
layout: post
title: "This Is How You Force iPhone to Power Off if It Get Stuck"
date: 2019-10-19T00:16:37+08:00
categories: []
---

When I say stuck, I mean **really stuck** -- when pressing the side button and volume button does NOT even work.

## My situation

I encountered this situation when building and installing an app from Xcode. As usual, the app launch screen is shown first. But something went wrong. It took way too long, and stuck at the launch screen.

Intuitively, I stop Xcode, pull out the USB cable, and try to reinstall again.

But, my **iPhone 11 Pro Max** does not respond any longer!

Ok, not totally unresponsive. In fact, it can do many things **except going to the springboard**. It can still access widgets, play music, volume button still works, use siri, camera, etc..

It just can't go to springboard and access apps.

## Failed Ways

I tried many ways to shut down the iPhone, including:

1. The designed trigger by holding all 3 available buttons on iPhone -- Somehow the "Slide to Power Off" never appears
2. Asking Siri to shut down -- Nope, Siri can't do that
3. Connect to mac -- But can't connect

The backup plan is to drain the battery, by playing loud music constantly with the screen on. Unfortunately, my battery was at 100%..

## The Ultimate Solution

Then an ingenious idea struck me, after trying for an hour.

1. Tell Siri to **Turn on assistive touch** -- that Siri can do
2. Use the on screen buttons to access **Device > Restart**

![](/images/turn-on-assistive-touch-restart.jpg)

Yeah.

_UPDATE: [hboon](https://twitter.com/samwize/status/1185382585100193792) gave a tip that here is a [hard reset sequence](https://support.apple.com/en-sg/guide/iphone/iph8903c3ee6/ios): press volume up (release) > press volume down (release) > press and hold side button. This will reset without needing the slider (TBC)._
