---
layout: post
title: "Paros - a mitmproxy with GUI for sniffing HTTP traffic"
date: 2012-09-15 01:47
comments: true
categories: iOS
---

I introduced [mitmproxy for sniffing iPhone HTTP traffic](http://blog.just2us.com/2012/05/sniff-iphone-http-traffic-using-mitmproxy/) back at [just2us.com](http://just2us.com).

[mitmproxy](http://mitmproxy.org/) is awesome, except that it isn't very user friendly, as it is a text console interface. Many times, I have to figure what are the keys to move around or access certain function.

That's where [Paros](http://www.parosproxy.org/) shines.

This is how you setup up Paros to sniff iPhone HTTP Traffic:

<!-- more -->

## 1. Install Paros ##

Paros runs on Java. [Download](http://www.parosproxy.org/download.shtml) for [install](http://www.parosproxy.org/install.shtml). 

For Mac, download the Linux version.

Unzip and run `paros.jar`.


## 2. Find out your machine IP ##

Find out the local IP for your machine. You need this for the next step.

For Mac, run the command `ifconfig en1` and copy down the IP.


## 3. Setup your iPhone proxy ##

Make sure your iPhone is on the same network as the machine running Paros. eg. connect to the same Wifi

On your iPhone, open **General** > **Wi-Fi** > Go to details for your Wifi network.

From there, choose **Manual** for **HTTP Proxy**. Enter the **machine IP** for Server and **8080** for Port.

{% img center /images/iphone-http-proxy-paros.png 360 iPhone network settings %}

In the screenshot above, my machine IP is 192.168.1.68.

## 4. Start running your target app and sniff ##

Use your iPhone to run the app that you want to sniff the traffic.

Paros interface will immediately show the URL sites the app connects to. The interface is pretty self explanatory so I won't go on from here.

Rock on from there!