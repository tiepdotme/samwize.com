---
layout: post
title: "Wifi Scanner for Mac OSX (Mountain Lion)"
date: 2012-10-28 10:05
comments: true
categories: Mac
---

Mountain Lion comes with a Wifi Scanner that is much better than you expected.

I tried out [KisMAC](http://kismac-ng.org/) at first, but couldn't get it working. I hate reading their lengthy [newbie guide](http://trac.kismac-ng.org/wiki/NewbieGuide).

Then I found a gem in Mountain Lion.

<!-- more -->

It turns out the Wi-Fi Diagnostics Tool (app) in Mountain Lion is updated with new features. It has added a wifi stumbler to discover the wifi nearbys (ssid, channel, protocols, etc).

To do a Wifi scan, 

1. Go to terminal and `open open /System/Library/CoreServices/Wi-Fi\ Diagnostics.app/`

2. Go to **File** > **Network Utilities**

3. Press Scan

{%img /images/wifi-scanner-mountain-lion.png Scan Results %}

That's it. You got to see the details of wifi nearby.

PS: I was looking for a uncongested, clear channel to use.