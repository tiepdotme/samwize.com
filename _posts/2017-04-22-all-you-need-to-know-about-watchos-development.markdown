---
layout: post
title: "All You Need to Know About watchOS Development"
date: 2017-04-22T14:31:36+08:00
categories: [watchOS]
---

This is everything that I wish I know about developing app on watchOS 3.

## Companion App

The companion app is the iOS app. Sometimes called the parent or host app.

- You CANNOT open companion app programmatically
- You can communicate with the companion app (see later under Communicating) and put the app in background state


## Setup bi-directional session

Setup a session between watch and companion app using [`WCSession`](https://developer.apple.com/reference/watchconnectivity/wcsession).

You need to setup either direction, or both directions.

The [delegate](https://developer.apple.com/reference/watchconnectivity/wcsessiondelegate) manages state change, and also receiving of messages.

On iOS, in your app delegate,



On watchOS, in your watch delegate,



## Ways to Communicate

There are a number of ways, and you [choose what is right](https://developer.apple.com/library/content/documentation/General/Conceptual/WatchKitProgrammingGuide/SharingData.html#//apple_ref/doc/uid/TP40014969-CH29-SW1) for your app.

I will mention what is easiest -- [`sendMessage`](https://developer.apple.com/reference/watchconnectivity/wcsession/1615687-sendmessage). Let's look at the signature:

    func sendMessage(_ message: [String : Any], replyHandler: (([String : Any]) -> Void)?, errorHandler: ((Error) -> Void)? = nil)

1. You send a message payload in the form of a dictionary with `sendMessage`
2. The other side recieve via [didReceiveMessage](https://developer.apple.com/reference/watchconnectivity/wcsessiondelegate/1615677-session) (one of `WKSessionDelegate` method)
3. The other side can reply via the `replyHandler`
4. You get back the reply in your reply handler

This is a simple send and reply mechanism, triggered by one side.


## Open Companion App in Active State

This is a very common scenario -- watch app wants to open companion (iOS) app.

That is NOT possible.

The best we developers can manage is this:

1. Watch app communicates via `sendMessage`
2. iOS app receives message
3. iOS app reply it's current state -- active or not
4. Watch app receives reply, and if state is not active, tell user to open the iOS app

The above can be improved.

1. When iOS app is opened, `sendMessage` to watch app. This is being "proactive".
2. Watch app receives message and ready to work with iOS app


## Debug BOTH watch and iOS targets

It seems like you cannot be running and debugging watchOS and iOS targets at the same time.

It is actually possible, but [not obvious](https://forums.developer.apple.com/thread/16003).

The steps:

1. Run watch app
2. Xcode > Debug > Attach to process > select your iOS app

If you want to debug your iOS app before it is being launched, you can "Attach to Process by PID or Name...".


## Pitfall: Xcode Build Errors

I encountered Xcode bug where the watch app build settings does not include the correct build architecture. Not sure why, but Xcode _always_ screw up with the build architecture.

Plus, Xcode throws confusing errors such as `WatchKit App doesn't contain any WatchKit Extensions. Verify that the value of NSExtensionPointIdentifier in your WatchKit Extension's Info.plist is set to com.apple.watchkit` and `Embedded Binary Validation utility Error`, but really, the root cause is that it didn't build the watch extension.

Make sure under Watch Extension target > Build Settings > Valid Architectures, it has `armv7k` (device) and `i386` (for simulator).

More technical notes can be found [here](https://developer.apple.com/library/content/technotes/tn2424/_index.html).
