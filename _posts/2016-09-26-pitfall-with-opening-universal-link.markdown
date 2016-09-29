---
layout: post
title: "Pitfall With Opening Universal Link"
date: 2016-09-26T10:59:41+08:00
categories: [iOS]
---

Universal link are familiar http links that iOS apps can now use to open apps, rather than opening the link in Safari web browser.

If the app is installed, then the universal should work just fine.

One pitfall is when the user **disassociate** the link; as if telling iOS that the link should NOT open the app.

We will explain how that happens and the solution.


## Opening a Universal Link

We use Uber app as an example.

```swift
let url = "https://m.uber.com/"
UIApplication.sharedApplication().openURL(url)
```

The above will open Uber app.


## The Pitfall

The pitfall is when the user tap on the top right **uber.com**.

![Open Universal Link](/images/uber-open-universal-link.jpg){:.border} 

Tapping on that will tell iOS that you, the user, do NOT want Uber app to be opened for the link.

To be specific, it only affects the app that opened it. Other apps will still open fine. The reason is for user to _disable_ certain apps. 


## The solution

When the universal link _doesn't work_, it will open Safari instead.

In the webpage, you have to **pull down** to reveal the _smart banner_ that will give you the option to re-open Uber app.

![Open Uber App, Again](/images/uber-in-safari.jpg){:.border} 

Tap on **OPEN**, and iOS will re-associate the universal link to work for your app.
