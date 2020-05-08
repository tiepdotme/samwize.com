---
layout: post
title: "iTunes File Sharing - Expose Documents Directory"
date: 2013-07-10 00:32
comments: true
categories: iOS
---

iOS introduced a way for users to transfer files from an app to a computer via iTunes.

It is simply adding `UIFileSharingEnabled` to info.plist. There is a very small introduction [here](http://developer.apple.com/library/ios/#technotes/tn2152/_index.html).

<!-- more -->

When you enable file sharing, ALL files in `<Application_Home>/Documents` will be shared.

Users can copy files to and from the app, as well as **delete**.

So be cautious of that.

You might also want to monitor the directory. There is [a way](http://www.mlsite.net/blog/?p=2312) to do that using `kqueue()`.

## Write to directory (in Swift)

The API is via `FileManager`.

```swift
// The Documents for every app
let documentDirectory = try! FileManager.default.url(for: .documentDirectory, in: .userDomainMask, appropriateFor: nil, create: false)

// Make sure you create any inner directories
let imageDirectory = documentDirectory.appendingPathComponent("/export/images/")
try! FileManager.default.createDirectory(at: imageDirectory, withIntermediateDirectories: true, attributes: nil)

// Write some data
try! data.write(to: imageDirectory.appendingPathComponent("lion.jpg"))
```

### Simulator how?

If you want to access the app container on the simulator, you need to:

1. Run on simulator
2. Pause, and `po NSHomeDirectory()`
3. Open the directory in Finder and do whatever you want :)
