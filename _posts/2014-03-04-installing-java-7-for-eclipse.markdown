---
layout: post
title: "Installing Java 7 for Eclipse"
date: 2014-03-04 22:40:57 +0800
comments: true
categories: [Java, AppEngine]
---

I encountered [this error](https://code.google.com/p/googleappengine/wiki/SdkForJavaReleaseNotes#Version_1.9.0_-_February_26,_2014) when deploying a Google AppEgine app:

> Java 6 applications cannot be deployed to Google App Engine from any version of the SDK... you can request that your application be whitelisted for Java 6 deployment from http://goo.gl/ycffXq.

<!-- more -->

And so I have to add Java 7 SDK to Eclipse.

This is what you have to do:

1. Download & install [Java 7 JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)

2. In Eclipse, go to Preferences > Java > Installed JREs > Add, then select the directory, which should be

    `/Library/Java/JavaVirtualMachines/jdk1.7.0_51.jdk/Contents/Home`

3. Tick the checkbox to make Java SE 7 the default

4. Go to Preferences > Java > Compiler > Compliance Level > Select 1.7

Test, and deploy!

There should not be any problem with migrating to Java 7.
