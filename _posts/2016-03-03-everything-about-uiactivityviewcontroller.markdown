---
layout: post
title: "Everything About UIActivityViewController"
date: 2016-03-03T15:38:47+08:00
categories: [iOS]
published: true
---

[UIActivityViewController](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIActivityViewController_Class/) was introduced in iOS 6 for apps to handle data (text, url, image, etc) in another app.

## Share a text and URL

One of the most common scenario is to share a URL, accompanied by some text. It could be accomplished with this code:

```swift
let textToShare = "Hello world"
let urlToShare = NSURL(string: "http://samwize.com")
let activityViewController = UIActivityViewController(activityItems: [textToShare, urlToShare!], applicationActivities: nil)
// Exclude irrelevant activities
activityViewController.excludedActivityTypes =
    [UIActivityTypeAssignToContact, UIActivityTypeSaveToCameraRoll, UIActivityTypePostToFlickr,
        UIActivityTypePostToVimeo, UIActivityTypeOpenInIBooks]
navigationController?.presentViewController(activityViewController, animated: true, completion: nil)
```


## UIActivityItemSource

Things are never so simple..

- Mail will not have a subject, because you never specify one
- In Message/Mail/etc, the content will have the text, followed by the URL (it looks bad if the text already contains the URL)

The better, more advanced way, is to implement [`UIActivityItemSource`](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIActivityItemSource_protocol/index.html
) for your view controller.

By implementing the protocol, you can change the string to returning, depending on what activity it is. You could even return different types (String/UIImage/etc).

```swift
func activityViewControllerPlaceholderItem(activityViewController: UIActivityViewController) -> AnyObject {
    return "Hello World"
}

func activityViewController(activityViewController: UIActivityViewController, itemForActivityType activityType: String) -> AnyObject? {
    switch activityType {

    case let x where [UIActivityTypePostToFacebook, UIActivityTypePostToTwitter].contains(x):
        // Returns text for social media services
        return "Sharing with you.. hello world!"

    case UIActivityTypeMail:
        // Returns long text for email
        return "Hello World~~~~~~~~~~~~~~~~~~"

    case let x where [UIActivityTypeCopyToPasteboard].contains(x):
        // Returns url
        let url = NSURL(string: "http://samwize.com")
        return url!

    default:
        return "Hello World"
    }
}

func activityViewController(activityViewController: UIActivityViewController, subjectForActivityType activityType: String?) -> String {
    return "Hello Subject"
}

func activityViewController(activityViewController: UIActivityViewController, thumbnailImageForActivityType activityType: String?, suggestedSize size: CGSize) -> UIImage? {
    return UIImage(named: "hello")
}
```

With that, you now init `UIActivityViewController` with your view controller.

```swift
let activityViewController = UIActivityViewController(activityItems: [self], applicationActivities: nil)
```

## Sharing directly to Facebook/Twitter using `SLComposeViewController`

Instead of using `UIActivityViewController`, which has the extra step of prompting user which app to use, [SLComposeViewController](https://developer.apple.com/library/prerelease/ios/documentation/NetworkingInternet/Reference/SLComposeViewController_Class/index.html) can be used if all you want is post to Facebook/Twitter/etc.


## Sharing directly using their url scheme

Another way to open an app to share data is via the app's url scheme.

For example, [WhatsApp](https://www.whatsapp.com/faq/en/iphone/23559013) provided a simple url scheme to send text:

    whatsapp://send?text=Hello%2C%20World!

Pitfall: Starting from iOS 9, Apple changed the way we can open URL. You need to add to `Info.plist` the key `LSApplicationQueriesSchemes`, which is an array containing the URL schemes that can be opened. For example, to open WhatsApp, add "whatsapp" to the array.

If your text to share includes a URL eg. "Visit http://samwize.com today!", then this URL encoding tips is for you:

```swift
let set = NSCharacterSet.URLQueryAllowedCharacterSet().mutableCopy() as! NSMutableCharacterSet
set.removeCharactersInString(":/?=")    // Encode these too!
guard let encodedShareText = shareText.stringByAddingPercentEncodingWithAllowedCharacters(set) else { return }
guard let url = NSURL(string: "whatsapp://send?text=\(encodedShareText)") else { return }

if UIApplication.sharedApplication().canOpenURL(url) {
    UIApplication.sharedApplication().openURL(url)
} else {
    // Cannot open WhatsApp
}
```


## `UIDocumentInteractionController`

To share an image to WhatsApp is [more difficult](http://stackoverflow.com/questions/23921990/sharing-image-to-whatsapp-facebook). You will need to use `UIDocumentInteractionController`.


## Other Notes

- Facebook [does not allow](http://stackoverflow.com/a/29891228/242682) the text (aka status) to be programatically changed when using `SLComposeViewController` or `UIActivityViewController`. However, if you use their [Facebook SDK](https://developers.facebook.com/docs/sharing/ios/share-button), then you can change the text..
