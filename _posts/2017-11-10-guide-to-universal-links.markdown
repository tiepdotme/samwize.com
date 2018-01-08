---
layout: post
title: "Guide to Universal Links"
date: 2017-11-10T11:32:18+08:00
categories: [iOS]
---

[Universal links](https://developer.apple.com/library/content/documentation/General/Conceptual/AppSearch/UniversalLinks.html) are HTTP links that open your app, instead of the website in Safari.

The old way is called deeplink -- where every app register it's own schema eg. `myapp://`

Universal links bring closer synergy between web and app with the familiar `https://`

## Step 1. apple-app-site-association JSON file

This **association file** (AASA) is to establish the trust between your domain/server/website with your app.

Among the 3 steps, this step is the most complicated.

Let's use an example of a `apple-app-site-association` file (note, there is no file extension):

```json
{
    "applinks": {
        "apps": [],
        "details": [
            {
                "appID": "9JA89QQLNQ.com.apple.wwdc",
                "paths": [ "/wwdc/news/", "NOT /videos/wwdc/2010/*", "/videos/wwdc/201?/*"]
            },
            {
                "appID": "ABCD1234.com.apple.messaging",
                "paths": [ "*" ]
            }
        ]
    }
}
```

The `details` array is a list, with implicit priority in matching the paths.

You can support multiple apps as each detail has it's own `appID`. The `appID` is made up of the team ID and the bundle ID.

The `paths` is a list of paths and:

- You can exclude paths by using `NOT` prefix
- The system evaluate each path in order 

## Step 1.5 (Optional) Sign it or not

If you only need universal link, this is optional.

But if you want to support [Shared Web Credentials](https://developer.apple.com/documentation/security/shared_web_credentials) and Handsoff, then you must sign the AASA file.

## Step 2. Upload to server

You must make the AASA file crawlable by Apple bot, and can either place in:

1. https://mydomain.com/apple-app-site-association
2. https://mydomain.com/.well-known/apple-app-site-association

The following is important, yet it is not stated in the Apple's guide (but you can find in the [troubleshooting FAQ](https://developer.apple.com/library/content/qa/qa1916/_index.html)):

> The AASA file must NOT be served through a 3xx redirect. Make sure this file is served without any redirects. 

And via HTTPS/SSL. And HTTP Content-type must be `application/json`.

~~[Validate with Apple Bot](https://search.developer.apple.com/appsearch-validation-tool), and **Link to Application** should pass.~~ Apple's tool is unreliable, so instead use [Branch.io's tool](http://branch.io/resources/aasa-validator/),or [this heroku app](https://limitless-sierra-4673.herokuapp.com).

## Step 3. Add entitlement in app

Go to your target > Capabilities > Associated Domains, and include the domains that your app wants to handle.

You MUST prefix with `applinks:` eg. `applinks:mydomain.com`

The system uses the longest substring matching.

When matched, it will trigger an NSUserActivity in your app delegate.

## Step 4. Handle NSUserActivity 

Implement [`application:continueUserActivity:restorationHandler:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623072-application) in your app delegate.

The same handler is used for Handsoff or Spotlight search. So how do you differentiate it is handling for tapping universal links? Look at `activityType` of [NSUserActivity](https://developer.apple.com/documentation/foundation/nsuseractivity).

Example:

```swift
func application(_ application: UIApplication, continue userActivity: NSUserActivity, restorationHandler: @escaping ([Any]?) -> Void) -> Bool {
        if userActivity.activityType ==  NSUserActivityTypeBrowsingWeb {
            // Handle universal link
            guard let url = userActivity.webpageURL else { return false }
            // Handle it..
            return true
        } else if userActivity.activityType == CSSearchableItemActionType {
            // Handle spotlight search
        }
        return false
```

## Step 5. Test it

Your integration test flow:

1. Uninstall your app
2. Install from Xcode (or TestFlight)
3. Test a universal link in eg Email, Notes

It seems like when you install your app during development, or beta from TestFlight, Apple bot will crawl the AASA file. That's the trigger. That's how you test universal links during development.

## When will it open app?

If you reach here, then your app should be working with universal links.

But there are many pitfalls with universal links.

Sometimes, it doesn't open as expected. Here is why it does NOT open your app:

- In Safari, if user enter the domain in URL search bar
- In Safari, clicking on the same domain
- When your app is opened, but user tap on the top left breadcrumb that "Opens Safari"

## More on the breadcrumb

iOS remembers the choice the user made when opening the universal link the first time.

Users could tap on the top left breadcrumb in status bar that says "Back to Safari", and iOS will remember that the user does NOT want to open that in app!

Afterwhich, iOS continues to open your website in Safari until the user chooses to open your app **by tapping OPEN in the Smart App Banner** on the webpage. See these [screenshots](https://stackoverflow.com/a/39694208/242682).

## A BIG Tip

Another way is **long pressing a link**. 

This will provide 2 options in a alert:

1. Open in Safari
2. Open in "Your App"

If you see (2), you know your universal link integration worked.

And now, whichever option the user chooses, iOS will remember it as The Choice.

## WKWebView/UIWebView

We know universal links when tapped in Safari or Messages or etc will open the app (if possible).

What about in web views in other apps?

Quoting my previous [guide in WKWebView](http://samwize.com/2016/06/08/complete-guide-to-implementing-wkwebview/):

> As noted in Apple Doc, iOS 9 users can tap on universal links in WKWebView, and it will open the app. It is the same for UIWebView and SFSafariViewController.

However, recently there seems to be a [hack](https://stackoverflow.com/a/44942814/242682) to NOT open the app. Use it to your advantage.


## Troubleshooting

It could be frustrating to troubleshoot, so here is a list [things to debug](http://building.usebutton.com/debugging/ios/deep-linking/links/universal-links/2016/03/31/debugging-universal-links/), more on the [paths in AASA](https://sailthru.zendesk.com/hc/en-us/articles/217102466-Universal-Links-Troubleshooting-and-FAQ), the previously mentioned non-Apple [AASA validation tool](https://limitless-sierra-4673.herokuapp.com), and good to know that it seems like the AASA file is fetched on the device everytime the app is [installed/updated](https://stackoverflow.com/a/35616335/242682), and then cached.

If still didn't work, at least know there are others who [hate](https://medium.com/mobile-growth/the-things-i-hate-and-you-should-know-about-apple-universal-links-5beb15f88a29) it too.

Try restarting your device too. It did happen to me once during development where it just won't fetch from any of the applinks domain, until I restart my phone.