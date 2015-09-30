---
layout: post
title: "Xcode Commands to build app and run on simulator"
date: 2015-03-11 18:18:07 +0800
comments: true
categories: [iOS]
---

There will be times where you want to do these on the command line.

## List the available SDKs

    xcodebuild -showsdks

For example, I have **iphonesimulator8.2** in my list of SDKs. This will be used in the next step.

<!-- more -->


## Xcode Build

Build the project:

    xcodebuild -project '/path/to/Awesome.xcproj' -arch i386  -sdk iphonesimulator8.2

If you are using workspace, then you need to provide the scheme too:

    xcodebuild -workspace '/path/to/Awesome.xcworkspace' -scheme 'Awesome-Production' -arch i386  -sdk iphonesimulator8.2

The build will be created at **~/Library/Developer/Xcode/DerivedData**.

So, if the build is successful, the app will be at **~/Library/Developer/Xcode/DerivedData/Awesome-dvxamtpiqakwogarovbwiagepzxj/Build/Products/Release-iphonesimulator/Awesome.app**. This path is needed in the next step.


## Run on Simulator

Use [ios-sim](https://github.com/phonegap/ios-sim) to help run on simulator. 

Install it with `brew install ios-sim`.

Then run:

    ios-sim launch <path-to-app>

That's it!
