---
layout: post
title: "Auto fill Username &amp; Password fields in UIWebView"
date: 2013-03-15 01:46
comments: true
categories: iOS
---

If you use `UIWebView` and would like to automatically fill in the username and password (or any other text input fields), you could do so by executing Javascript on the webview.

I first learnt about this from [stackoverflow](http://stackoverflow.com/a/9722805/242682), and has edited slightly.

The technique is to implement [`UIWebViewDelegate`](http://developer.apple.com/library/ios/#documentation/uikit/reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intf/UIWebViewDelegate) and execute some Javascript in `webViewDidFinishLoad`. 

```objc
- (void)webViewDidFinishLoad:(UIWebView *)webView {
    // Auto fill the username and password text fields, assuming the HTML has
    // <input type="text" name="username"> and
    // <input type="text" name="password">
    NSString *savedUsername = [[NSUserDefaults standardUserDefaults] objectForKey:@"USERNAME"];
    NSString *savedPassword = [[NSUserDefaults standardUserDefaults] objectForKey:@"PASSWORD"];
    if (savedUsername.length != 0 && savedPassword.length != 0) {
        // Create js strings
        NSString *loadUsernameJS = [NSString stringWithFormat:@"var inputFields = document.querySelectorAll(\"input[name='username']\"); \
                                    for (var i = inputFields.length >>> 0; i--;) { inputFields[i].value = '%@';}", savedUsername];
        NSString *loadPasswordJS = [NSString stringWithFormat:@"var inputFields = document.querySelectorAll(\"input[name='password']\"); \
                                    for (var i = inputFields.length >>> 0; i--;) { inputFields[i].value = '%@';}", savedPassword];
        // Runs the JS
        [self.webview stringByEvaluatingJavaScriptFromString: loadUsernameJS];
        [self.webview stringByEvaluatingJavaScriptFromString: loadPasswordJS];
    }
}
```