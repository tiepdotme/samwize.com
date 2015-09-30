---
layout: post
title: "Pitfall - If Xcode does not show simulators to run, or think it is a Mac app"
date: 2015-01-16 14:13:05 +0800
comments: true
categories: [iOS]
---

A pitfall if you have 1 of these strange encounters in Xcode:

- When trying to run an app on simulator, you just couldn't because it won't list the simulators (but you are sure you have working simulators)!

- You have an iOS app, but Xcode thinks it is a Mac app when you try to run.

If you dig into the target/project schemes, and is still clueless, I think I could help.

<!-- more -->

Close Xcode.

Right-click on your `xcodeproj file` and select "show package contents". 

Delete everything inside the `xcuserdata` folder.

Open the project in Xcode.

If it works, good! 

Xcode somehow screwed up. 

_PS: Xcode did screwed up while I was running Xcode 6 beta and trying to run their sample code - Lister app._
