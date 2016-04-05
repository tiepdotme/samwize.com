---
layout: post
title: "About App Link (branch.io)"
date: 2016-04-05T09:43:39+08:00
categories: [iOS]
---

App Link is, technically, not a link, but metadata (eg. `og:title` and `og:url`) on a webpage.

It is called an App Link, because the webpage (with a URL link) usually open/install the app.

It is pretty interesting to learn about App Link for iOS, and specifically, I wil be [introducing branch.io](https://bnc.lt/m/imsbo8Ilis), which is a free deep linking service for both iOS and Android. And they claimed to be [FREE forever](https://branch.io/pricing/)!


## Who else provides App Links?

[Facebook](https://developers.facebook.com/docs/applinks/ios) and [Google](https://developers.google.com/app-invites/) has their own service to create app links.

But they both required the user to login to Facebook/Google, which is a big disadvantage.

On the other hand, branch.io is neutral, and is very focused in doing what they do best.


## Social Media Description

If you have an app, but doesn't have a website, branch.io will be a great help for you to generate a link which you can share on social media platforms.

Behind the scene, the link generates the [open graph](http://ogp.me) metadata which describes your app.


## App Link passes data over to app

This feature is the biggest reason why you should use branch.io.

We all know that when a user install an app from the App Store, no data can be passed to the app. eg. You won't know the source/url that resulted in the user downloadong the app.

branch.io solved this problem by using [fingerprinting](https://blog.branch.io/deferred-deep-link-for-facebook-install-ads). In short, it identifies a user with **IDFA**, then match it back after the app is installed. This is not perfect, but works 99%.


## Setting up branch.io

The [SDK integration guide](https://dev.branch.io/getting-started/sdk-integration-guide/guide/ios/) is good.

The part on [Universal and App Links](https://dev.branch.io/getting-started/universal-app-links/overview/) is required too. They also provided good explanation on [what's changed in iOS 9.2](https://dev.branch.io/getting-started/universal-app-links/support/ios/#what-changed-in-ios-9-and-92). In short, Apple does not seamlessly open the app starting with iOS 9.2. Rather, it shows an alert and require the user to explicitly confirm first.

The code needed in `AppDelegate`:

```swift
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
    Branch.getInstance().initSessionWithLaunchOptions(launchOptions, andRegisterDeepLinkHandler: handleBranch)
}

func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject) -> Bool {
    Branch.getInstance().handleDeepLink(url)
}
 
func application(application: UIApplication, continueUserActivity userActivity: NSUserActivity, restorationHandler: ([AnyObject]?) -> Void) -> Bool {
    return Branch.getInstance().continueUserActivity(userActivity)
}

// This branch handler will run when:
// - App Start
// - After `application:openURL:...`
// - After `application:continueUserActivity:...`
//
// Note the params will be "consumed". After the first run, on the subsequent, it will have `clicked_branch_link` false, `is_first_session` false
func handleBranch(params: [NSObject: AnyObject]!, error: NSError!) {
    if let error = error {
        print("Error: \(error)")
    }
    print("Branch Params: \(params)")
    // Handle it
}
```

Note: I am not very sure about this [guide](https://dev.branch.io/getting-started/universal-app-links/advanced/ios/#how-to-handle-old-uri-paths-with-universal-links), which says to prevent "deep linked twice". It has a flag, which is set to ignore in the entry point for iOS 8.x `application:openURL:...`.


## Handling Push

Branch can also handle the push notification payload, with the branch link url in key `branch`.

```swift
func application(application: UIApplication, didReceiveRemoteNotification launchOptions: [NSObject: AnyObject]?) -> Void {
    Branch.getInstance().handlePushNotification(launchOptions)
}
```
