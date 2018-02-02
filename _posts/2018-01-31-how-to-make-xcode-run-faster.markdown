---
layout: post
title: "How to Make Xcode Run Faster"
date: 2018-01-31T15:34:12+08:00
categories: [Xcode]
published: false
---

https://github.com/fastred/Optimizing-Swift-Build-Times

http://developear.com/blog/2016/12/30/Speed-Swift-Compilation.html


//:configuration = Debug
OTHER_SWIFT_FLAGS = $(inherited) -DDEBUG -Xfrontend -debug-time-function-bodies -Xfrontend -warn-long-function-bodies=100 -Xfrontend -warn-long-expression-type-checking=100 -Onone

//:completeSettings = none
