---
layout: post
title: "NSAttributedString, HTML and Unicode Encoding"
date: 2016-05-24T17:09:57+08:00
categories: [iOS]
---

While converting a HTML string to a `NSAttributedString`, I discovered a perculiar thing about it's unicode encoding format.

## HTML as NSAttributedString 

Converting HTML `String` to `NSAttributedString` can be easily accomplished with this String extension:

```swift
extension String {
    func htmlAttributedString() -> NSAttributedString? {
        guard let data = self.dataUsingEncoding(NSUTF16StringEncoding, allowLossyConversion: false) else { return nil }
        guard let html = try? NSMutableAttributedString(
          data: data, 
          options: [NSDocumentTypeDocumentAttribute: NSHTMLTextDocumentType], 
          documentAttributes: nil) else { return nil }
        return html
    }
}
```

To use,

```swift
label.attributedText = "<b>Hello</b> \u{2022} World".htmlAttributedString()
```

In the above, I have purposely added a unicode **\u2022** to show that it renders unicode correctly.



## NSAttributedString uses NSUTF16StringEncoding

If you observe the code carefully, when we call `dataUsingEncoding`, we use `NSUTF16StringEncoding`.

_Sidetrack: A small introduction to unicode and encoding will be in next section._

This is because `NSAttributedString` requires data to be encoded in **UTF-16**.

NOT UTF-8, as I expected at first.

Also note that `NSUnicodeStringEncoding` is the same as `NSUTF16StringEncoding`.

While the [documentation](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSAttributedString_Class/) never specify what encoding `data` has to be, we can get a hint from
`NSString` which is [conceptually UTF-16](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Strings/Articles/stringsClusters.html).

Another way is to specify the encoding in NSAttributedString initializer:

```swift
NSMutableAttributedString(
  data: data, 
  options: [NSDocumentTypeDocumentAttribute: NSHTMLTextDocumentType, 
  NSCharacterEncodingDocumentAttribute: NSUTF8StringEncoding], 
  documentAttributes: nil) else { return nil }
```

In the above, `data` will have to be UTF-8 encoded.



## An Introduction to Encoding Strings 

Why do we need to encode?

We need to handle encoding/decoding of string when it is **converted to data** (eg `NSData`). 

When writing to a file, ultimately, the string representation is written as bytes of data. This conversion of string to data requires an encoding form.

This is because 1 character in a string (or specifically unicode string) can be represented by 1-4 bytes, depending on the encoding form. 

We discuss 2 of the most common forms here.

A **UTF-16** character is minimumly 2 bytes (or 16 bits). 

But a [unicode character is up to 21 bit](http://unicode.org/faq/utf_bom.html), so while UTF-16 can represent most of the unicode characters (specifically 63K characters) in 2 bytes, there are characters that it needs more than 2 bytes to represent. 

A **UTF-8** character is minimumly 1 byte (or 8 bits). Only 128 characters are encoded in 1 byte (this includes the very common ASCII). This is much more space efficient than UTF-16.

That's it for an introduction.

For more, read about how [Swift deal with String](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/StringsAndCharacters.html) and from [objc.io](https://www.objc.io/issues/9-strings/unicode/).

