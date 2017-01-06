---
layout: post
title: "Complete Guide to Implementing WKWebView"
date: 2016-06-08T10:38:53+08:00
categories: [iOS]
---

Using web view is not new. We had `UIWebView` for many years.

Then came `WKWebView` in iOS 8, and we had to re-learn some stuff.

This is a complete guide (in Swift!) to implementing your view controller that has a `WKWebView`, for the purpose of (duh) displaying webpages.

It is not as simple as I thought it would be, with a couple of lessons I learnt along the way.


## Setting up with Interface Builder

Currently (as of Xcode 7.3.1), you cannot add a `WKWebView` to your scene directly. There is a web view component in Object Library, but that is for the old `UIWebView`.

I am sure the component will be added someday, but for now, you have to setup in your code.

```swift
class MyWebViewController: UIViewController {

    var webView: WKWebView!

    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Create WKWebView in code, because IB cannot add a WKWebView directly
        webView = WKWebView()
        webView.translatesAutoresizingMaskIntoConstraints = false
        view.addSubview(webView)
        view.addConstraints(NSLayoutConstraint.constraintsWithVisualFormat("|-[webView]-|",
            options: NSLayoutFormatOptions(rawValue: 0),
            metrics: nil,
            views: ["webView": webView]))
        view.addConstraints(NSLayoutConstraint.constraintsWithVisualFormat("V:|-20-[webView]-|",
            options: NSLayoutFormatOptions(rawValue: 0),
            metrics: nil,
            views: ["webView": webView]))
            
        // 2 ways to load webpage: `loadHTML()` or `loadURL()`        
    }
}
```

In `viewDidLoad`, we have to create `WKWebView` in code. 

We add it to the view, flushing to the bounds, using [auto layout visual language format](/2014/05/13/first-taste-with-visual-language-format/).

There are 2 ways to load a webpage:

1. Load HTML as String 
2. Load URL request

In `viewDidLoad`, call either `loadHTML()` or `loadURL()`, which we gonna explore next.


## 1. Load HTML as String

```swift
func loadHTML() {
    webView.loadHTMLString("<html><body>"
        + "<p><a href=\"http://samwize.com\">http://samwize.com</a></p>"
        + "</body></html>", baseURL: nil)
}
```

We use `loadHTMLString` to load a local HTML.

Run this now, tap on the link, and nothing happens.

Why?

Because **App Transport Security policy** requires the use of a secure connection, or unless you whitelist it. Add the following to your `Info.plst`:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

### How did I know of the error?

Glad you ask. 

While Xcode debugger does not show up any error/warning, there is an the awesome **Safari Debugger**. 

Go to **Safari > Develper > Simulator/Your Device**, and you will be able to debug the page just like on desktop. 

For a real device, you will need to enable the feature in **Settings app > Safari > Advanced**.

Keep in mind of this VERY useful tool.


## 2. Load URL request

Simply using `loadRequest`.

```swift
func loadURL() {
    let urlString = "http://samwize.com"
    guard let url = NSURL(string: urlString) else {return}
    let request = NSMutableURLRequest(URL:url)
    webView.loadRequest(request)
}
```

## The 2 delegates

There are 2 delegates to the web view which you will most likely use.

```swift
class MyWebViewController: UIViewController, WKUIDelegate, WKNavigationDelegate {
    override func viewDidLoad() {
        ...
        webView.UIDelegate = self
        webView.navigationDelegate = self
    }
}
```

[WKUIDelegate](https://developer.apple.com/library/ios/documentation/WebKit/Reference/WKUIDelegate_Ref/) provides the method for presenting some native user interfaces (see Javascript dialog boxes later).

[WKNavigationDelegate](https://developer.apple.com/library/ios/documentation/WebKit/Reference/WKNavigationDelegate_Ref/) track navigations from page to page.



## Change URL Request HTTP Header

You can change a HTTP header value with a `NSMutableURLRequest` eg. in `loadURL()` like this:

```swift
request.setValue("Wolverine", forHTTPHeaderField: "X-Men-Header")
```

One of the most common header field is the **User-Agent**. You can set it on a request object.

But a more convenient way is using `webView.customUserAgent` to set just once for the web view. Or you can change it [globally by setting NSUserDefaults](http://stackoverflow.com/a/27331026/242682).

And this is how you find out the current user agent:

```swift
webView.evaluateJavaScript("navigator.userAgent") { (result, error) -> Void in
    print("User-Agent: \(result)")
}
```

## Go Back/Forward and Progress

You can navigate the history with the web view `goBack()` and `goForward()`.

To track the progress of loading a webpage, you will need to _observe_ some key paths. Add this in `viewDidLoad`

```swift
let webViewKeyPathsToObserve = ["loading", "estimatedProgress"]
for keyPath in webViewKeyPathsToObserve {
    webView.addObserver(self, forKeyPath: keyPath, options: .New, context: nil)
}
```

Then as the values change:

```swift
override func observeValueForKeyPath(keyPath: String?, ofObject object: AnyObject?, change: [String : AnyObject]?, context: UnsafeMutablePointer<Void>) {
    guard let keyPath = keyPath else { return }
    
    switch keyPath {
      
    case "loading":
        // If you have back and forward buttons, then here is the best time to enable it
        backButton.enabled = webView.canGoBack
        forwardButton.enabled = webView.canGoForward
        
    case "estimatedProgress":
        // If you are using a `UIProgressView`, this is how you update the progress
        progressView.hidden = webView.estimatedProgress == 1
        progressView.progress = Float(webView.estimatedProgress)
        
    default:
        break
    }
}
```

Lastly, when page is loaded, reset the progress view.

```swift
func webView(webView: WKWebView, didFinishNavigation navigation: WKNavigation!) {
    progressView.setProgress(0.0, animated: false)
}
```

## Print the HTML text

```swift
webView.evaluateJavaScript("document.documentElement.outerHTML.toString()", completionHandler: { (html: AnyObject?, error: NSError?) in
    print(html)
})
```

## Pitfall: Handling Javascript Dialog Boxes

This is often omitted in a custom implementation of `WKWebView`, yet it is very important.

Unlike Safari, `WKWebView` left out how the [3 Javascript dialog boxes](http://www.tutorialspoint.com/javascript/javascript_dialog_boxes.htm) are handled. 

You MUST implement the methods in `WKUIDelegate` to have a proper working web browser.

Here, we have a simple implementation by using `UIAlertController` to show the dialog boxes.

```swift
/// Handle javascript:alert(...)
func webView(webView: WKWebView, runJavaScriptAlertPanelWithMessage message: String, initiatedByFrame frame: WKFrameInfo, completionHandler: () -> Void) {
    ...
    let okAction = UIAlertAction(title: Okay, style: .Default) { _ in
        completionHandler()
    }
    ...
}

/// Handle javascript:confirm(...)
func webView(webView: WKWebView, runJavaScriptConfirmPanelWithMessage message: String, initiatedByFrame frame: WKFrameInfo, completionHandler: (Bool) -> Void) {
    ...
    let okAction = UIAlertAction(title: Okay, style: .Default) { _ in
        completionHandler(true)
    }

    let cancelAction = UIAlertAction(title: Cancel, style: .Cancel) { _ in
        completionHandler(false)
    }
    ...
}

/// Handle javascript:prompt(...)
func webView(webView: WKWebView, runJavaScriptTextInputPanelWithPrompt prompt: String, defaultText: String?, initiatedByFrame frame: WKFrameInfo, completionHandler: (String?) -> Void) {
    ...
    alertController.addTextFieldWithConfigurationHandler { (textField) in
        textField.text = defaultText
    }
    
    let okAction = UIAlertAction(title: Okay, style: .Default) { action in
        let textField = alertController.textFields![0] as UITextField
        completionHandler(textField.text)
    }

    let cancelAction = UIAlertAction(title: Cancel, style: .Cancel) { _ in
        completionHandler(nil)
    }
    ...
}
```

In the code above, I have left out the not-so-important part of creating [UIAlertController](http://nshipster.com/uialertcontroller/), adding the actions, and presenting it.

What's important are the `UIAlertAction` and the `completionHandler` to call back with.


## Pitfall: Unsupported URL

Custom scheme URL is not supported by `WKWebView` (but Safari will work). 

You can make it work by handling the "error":

```swift
func webView(webView: WKWebView, didFailNavigation navigation: WKNavigation!, withError error: NSError) {
    handleError(error)
}

func webView(webView: WKWebView, didFailProvisionalNavigation navigation: WKNavigation!, withError error: NSError) {
    handleError(error)
}

func handleError(error: NSError) {
    if let failingUrl = error.userInfo["NSErrorFailingURLStringKey"] as? String {
        if let url = NSURL(string: failingUrl) {
            let didOpen = UIApplication.sharedApplication().openURL(url)
            if didOpen {
                print("openURL succeeded")
                return
            }
        }
    }
}
```

Note: In iOS 9, you have to [whitelist](http://useyourloaf.com/blog/querying-url-schemes-with-canopenurl/) the URL schemes if you use `canOpenURL`, therefore we simply go ahead and `openURL`, then use the returned boolean.


## Bonus: Universal Links

Universal links are `http://...` URL that will open an app. They are similar to custom URI scheme to open app, but using regular http addresses. Yet it was claimed to be [untested software from Apple](https://blog.branch.io/apples-universal-links-a-testament-to-untested-software).

It is quite a tricky technology.

As noted in Apple Doc, iOS 9 users can [tap on universal links in `WKWebView`](https://developer.apple.com/library/ios/documentation/General/Conceptual/AppSearch/UniversalLinks.html), and it will open the app. It is the same for `UIWebView` and `SFSafariViewController`.

There is no way you can prevent a universal link from opening an app (if installed) when the link is in a page in your web view. [Google Chrome app](http://blog.tapstream.com/google-changes-chrome-to-prevent-abusive/) is the same. It was [clarified](http://blog.tapstream.com/why-has-google-broken-deeplinking-on-android/#comment-1906522285) by their engineer that deep/universal link will still be opened if it results from a user's tap.

[Branch](https://dev.branch.io/getting-started/universal-app-links/support/ios/#appsbrowsers-that-support-universal-links) provided a good guide on when universal link will **NOT** open the app:

1. When the link is entered into the browser address field (thus creating the intial web view)
2. Same domain
3. Javascript `onload()` or `click()`

In other words, it **only works** if the link is cross domain and is user driven (clicking on a `<a>` tag).

If a universal link sometimes did not work, then it is likely the OS has remembered your preference for the link. You have to ["reset"](http://stackoverflow.com/a/32745272/242682) it.

If you want to exclude a path, you can use a [`NOT` keyword](https://forums.developer.apple.com/thread/24981).
