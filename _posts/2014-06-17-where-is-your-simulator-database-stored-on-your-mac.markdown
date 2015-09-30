---
layout: post
title: "Where is your Simulator database stored on your Mac?"
date: 2014-06-17 13:51:18 +0800
comments: true
categories: iOS
---

There are times when you want to have the same data across your simulator and devices.

It is possible to copy and paste any data eg sqlite database and documents.

<!-- more -->

## Device data

You can use iExplorer to access the data on your real iPhone/iPad.

Go to the app > Library > Application Support > APP_NAME > and the sqlite data will be there.


## Simulator

For [simulators](http://stackoverflow.com/a/3495426/242682), they are in

    ~/Library/Application Support/iPhone Simulator/[OS version]/Applications/[appGUID]/

You can copy the data from device to simulator by simply copy and paste.

For example, I can copy the sqlite file to:

    ~/Library/Application Support/iPhone Simulator/7.1-64/Applications/9445759E-34C7-4080-9DC7-9A74032FCA60/Library/Application Support/Awesome App/


_UPDATE: Starting from iOS 8, the simulator data has moved_

It is now at:

    ~/Library/Developer/CoreSimulator/Devices/<DEVICE_GUID>/data/Containers/Data/Application/<APP_GUID>/Library/Application Support/AwesomeApp/

## Code to print the path

Perhaps, more conveniently and robust, you could add this code:

```objc
#if TARGET_IPHONE_SIMULATOR
    NSLog(@"Application Support Directory: %@", [[[NSFileManager defaultManager] URLsForDirectory:NSApplicationSupportDirectory inDomains:NSUserDomainMask] lastObject]);
#endif
```