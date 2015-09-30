---
layout: post
title: "POST Request to UIWebView"
date: 2013-03-15 01:58
comments: true
categories: iOS
---

In the last post, I wrote about [how you can automatically fill in username and password in a UIWebView](/2013/03/15/auto-fill-username-and-password-fields-in-uiwebview/).

This post, I will teach you how you can automatically login to a `UIWebView`.

It turns out to be simple if the login uses a simple form POST.

```objc
- (void)login {
    // Setup the URL
    NSString *loginUrl = @"https://just2us.com/login";
    NSURL *url = [NSURL URLWithString:loginUrl];    
    NSMutableURLRequest *requestObj = [NSMutableURLRequest requestWithURL:url];
    
    // POST the username password
    [requestObj setHTTPMethod:@"POST"];
    NSString *postString = [NSString stringWithFormat:@"username=%@&password=%@", @"samwize", @"secret"];
    NSData *data = [postString dataUsingEncoding: NSUTF8StringEncoding]; 
    [requestObj setHTTPBody:data];
    
    // Load the request
    self.webview.delegate = self;
    [self.webview loadRequest:requestObj];
}
```