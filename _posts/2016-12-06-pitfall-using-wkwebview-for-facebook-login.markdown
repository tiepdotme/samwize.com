---
layout: post
title: "Pitfall: Using WKWebView for Facebook Login"
date: 2016-12-06T17:51:26+08:00
categories: [iOS]
---

When using `WKWebView` (or `UIWebView`), we are actually using only 1 window.

For website that has Facebook login, it _might_ open a new window (a popup) for the user to login. Once login is completed, this popup window will close, and it will use an obscure method to pass a message to the original window.

The message has a URL like the following:

    https://m.facebook.com/v2.7/dialog/oauth?access_token=...&redirect_uri=...


## Pitfall of 1 Window

The [pitfall](http://stackoverflow.com/q/8025082/242682) is that when we use `WKWebView`, by default, we have only 1 window.

A naive implementation to `WKUIDelegate` is usually to load a request in that same web view:

```swift
extension WebViewController: WKUIDelegate {
    // Handles when a new frame/window is to be opened
    func webView(webView: WKWebView, createWebViewWithConfiguration configuration: WKWebViewConfiguration, forNavigationAction navigationAction: WKNavigationAction, windowFeatures: WKWindowFeatures) -> WKWebView? {
        webView.loadRequest(navigationAction.request)
        return nil
    }
}
```

When Facebook login wants to pass that message to the original window, it could NOT find that window, and so therefore it get stuck in the page (usually a white screen of nothing).


## Handling Multiple Windows

The solution is to support multiple windows.

```swift
func webView(webView: WKWebView, createWebViewWithConfiguration configuration: WKWebViewConfiguration, forNavigationAction navigationAction: WKNavigationAction, windowFeatures: WKWindowFeatures) -> WKWebView? {
    // nil means new window
    if navigationAction.targetFrame == nil {
        // A omit `createWebView`. It creates a new web view and add to your view.
        let newWebView = createWebView(withConfiguration: configuration)
        return newWebView
    }
    
    webView.loadRequest(navigationAction.request)
    return nil
}
```

I did not include what happens after this new web view is shown. For Facebook login case, you will probably want to remove the new web view in `webView:didFinishNavigation:`.


## Edge Case: Error 999

In the code above, there is a weird scenario where some website will load  `https://m.facebook.com/v2.7/dialog/oauth?access_token=...&redirect_uri=...` and result in `didFailNavigation`

A simple solution is to reload, after a few seconds. 
