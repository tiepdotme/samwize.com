---
layout: post
title: "Continue Network Fetching Even When App Goes to Background"
date: 2016-10-16T15:26:25+08:00
categories: [iOS]
---

## Problem

When an iOS app goes to background, network operations (GET/POST/Whatever) will be paused.

For long running network operations, this is a problem because the the operation is paused, and the app most likely at some point in time will be killed by the OS.

Then you have to start all over again.


## Solution

iOS provides a way to register your long running tasks, so that it gets another minute or more in the background.

The [`beginBackgroundTaskWithExpirationHandler`](https://developer.apple.com/reference/uikit/uiapplication/1623031-beginbackgroundtaskwithexpiratio) is the magic.

Let's see the code to using it:

```swift
func registerBackgroundTask() {
  // 1. Register task with expiration handler
  let backgroundTask = UIApplication.sharedApplication().beginBackgroundTaskWithExpirationHandler {
    // 2. Code to handle if takes way too long
  }

  // 3. Code for your long running task (synchronous case)

  // 4. End task
  UIApplication.sharedApplication().endBackgroundTask(backgroundTask)
}

```


## More than just network operations

While the situation cited in this post is on network operations, you should realize that any long running tasks can be registered.

For example, if you have an image processing task that could take minutes, it can be extended as well.