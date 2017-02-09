---
layout: post
title: "React Native Tips & Tricks"
date: 2017-01-13T15:12:18+08:00
categories: [React Native]
---

The previous post was the first React Native post on this website. It is an introduction in consideration to a native iOS Swift developer.

But there is so much to cover, because a JavaScript world has many discrete projects extending it's power.

This is a guide of all the tips & tricks, in no particular order.

## Wrapping Native API

React Native only provides a small set of API to the native SDK. You can [wrap](https://facebook.github.io/react-native/docs/native-modules-ios.html) them on your own, when something is missing.

[wix's react-native-invoke](https://github.com/wix/react-native-invoke) is another way that invokes native API using reflection.


## Navigation

Use [wix's navigation](https://github.com/wix/react-native-navigation/), which is using the native component `UINavigation`. React Native offical NavigtionIOS is similar, but they did not maintain it, so wix's is contributing well to the ecosystem.


## Building Release

    react-native bundle  --dev false --entry-file index.ios.js \
        --bundle-output main.jsbundle