---
layout: post
title: "Pitfall With UI Testing Record Button Disabled"
date: 2016-02-24T22:28:36+08:00
categories: [iOS]
---

I encountered a weird situation whereby the record button (at the bottom left of Xcode), is disabled.

UI Testing target is created, and the mouse cursor is at the UI Testing file. However, the record button is just disabled.

The solution is to go to **Product > Test**, so that the target is build first.

When it is setup correctly, you will find the test cases in your target scheme.

## Another error

It is also possible that you encountered an error when running the test:

> The bundle XXX couldn’t be loaded because it doesn’t contain a version for the current architecture. Try installing a universal version of the bundle.

This is likely because the **Valid Architecture** for the UI Testing target is set wrongly when the target was created.

Make sure you set the same architectures as your app target eg. `armv7`, `armv7s`, `arm64`
