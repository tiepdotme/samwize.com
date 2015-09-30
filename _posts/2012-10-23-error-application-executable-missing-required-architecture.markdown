---
layout: post
title: "Error: Application Executable Missing Required Architecture"
date: 2012-10-23 22:20
comments: true
categories: iOS
---

I swear this is the most common error I have seen whenever I upload an iOS binary.

> Error: application executable is missing a required architecture armv6

It occurs every time after Apple release a new iPhone (or architecture type).

<!-- more -->

In 2011, when I was using Xcode 4.2, when I think iPhone 4 was out, I need to [change](http://stackoverflow.com/a/8538393/242682) the Architectures settings from `armv7` to `armv6 armv7`.

There is another similar due to to "no architectures to compile for" which you can [solve](http://stackoverflow.com/a/5294634/242682) by changing `VALID_ARCHS`.

Now in 2012, with Xcode 4.5, with the new iPhone 5, and the phasing out of armv6 (used in the old generation of iPod and iPhone), it once again introduced the error.

This time, you have to [set](http://stackoverflow.com/a/12524366/242682) the deployment target to at least iOS 4.3.

Well done Apple.