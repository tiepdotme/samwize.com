---
layout: post
title: "Android Developement Refresher 2013"
date: 2013-04-24 23:13
comments: true
categories: [Android]
---

It has been nearly 3 years since I stopped Android development. 

A lot has changed since the days of version 1.6. Now it is 4.x!

Trying to do a little bit of compiling and setting up stuff these days, and I had some hiccups. This is a refresher guide on some topics.

<!-- more -->

## App Signing ##

The [long guide](http://developer.android.com/tools/publishing/app-signing.html) from Google didn't explictly said:

- When you build your app, it will be automatically signed with your `debug.keystore`, which will be created in `/bin/myapp.apk`.

- You can take that SIGNED apk and install on any real devices

- Don't be [confused](http://stackoverflow.com/q/4835925/242682) with `Export Unsigned ..` option. Even if the device "allow installation of non-Marketed applciation", the apk must be SIGNED

- Linking all up, you can take the SIGNED apk from `/bin/myapp.apk` and email to your real device to install


## Maps ##

No longer a Map View class. 

You have to read the [Android guide](http://developer.android.com/google/play-services/maps.html), and [setup Google Maps API V2 key](https://developers.google.com/maps/documentation/android/start#installing_the_google_maps_android_v2_api).

