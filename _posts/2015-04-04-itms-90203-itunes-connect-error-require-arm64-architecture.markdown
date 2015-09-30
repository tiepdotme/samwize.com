---
layout: post
title: "ITMS-90203 iTunes Connect Error: Require arm64 architecture"
date: 2015-04-04 20:58:43 +0800
comments: true
categories: [iOS]
---

When I tried to publish an app with Watch Kit App/Extension, I have the error:

> ERROR ITMS-90203: "Invalid architecture: Apps that include an app extension and a framework must support arm64

<!-- more -->

You can verify with `lipo` on your binary.

    lipo -info "/Users/junda/Library/Developer/Xcode/Archives/2015-04-04/AwesomeApp 4-4-15 8.49 pm.xcarchive/Products/Applications/AwesomeApp.app/AwesomeApp" 

It must list arm64.

If not, make sure all your targets includes arm64 in **Build Settings > Architectures** and also **Valid Architectures**. 

The standard architecture is **armv7** and **arm64**. 

NOTE: It is NOT **armv64**, it is **arm64**. A typo will result in not building for arm64 too.

When you build/archive, disconnect any connecting device too, because that could trigger "Build Active Architecture Only".

