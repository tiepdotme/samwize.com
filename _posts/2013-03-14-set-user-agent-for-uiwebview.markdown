---
layout: post
title: "Set User Agent for UIWebView"
date: 2013-03-14 23:46
comments: true
categories: iOS
---

There is a very easy (but not documented) way to set the User Agent header for HTTP requests sent via `UIWebView`.

I find saw the solution from [mphweb](http://www.mphweb.com/en/blog/easily-set-user-agent-uiwebview).

Basically, you just set register it with `NSUserDefaults`.

```objc
+ (void)initialize {
    // Set user agent (the only problem is that we can't modify the User-Agent later in the program)
    NSDictionary *dictionnary = [[NSDictionary alloc] initWithObjectsAndKeys:@"Your desired user agent", @"UserAgent", nil];
    [[NSUserDefaults standardUserDefaults] registerDefaults:dictionnary];
    [dictionnary release];
}
```