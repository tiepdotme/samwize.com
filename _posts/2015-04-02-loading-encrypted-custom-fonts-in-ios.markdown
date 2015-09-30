---
layout: post
title: "Loading (Encrypted) Custom Fonts in iOS"
date: 2015-04-02 21:14:11 +0800
comments: true
categories: [iOS]
---

We talked about [adding and using custom font](http://samwize.com/2012/09/14/adding-and-using-custom-font-in-ios/) back in 2012.

This is another way now with Core Text framework.

It also helps to load encrypted fonts which could be required by some of the font license.

<!-- more -->

## Loading Custom Font

```objc
#import <CoreText/CoreText.h>

- (void)loadCustomFont {
    NSString *fontPath = [[NSBundle mainBundle] pathForResource:@"JOURNAL" ofType:@"TTF"];    
    CFErrorRef error;
    CGDataProviderRef provider = CGDataProviderCreateWithFilename([fontPath UTF8String]);
    CGFontRef font = CGFontCreateWithDataProvider(provider);
    if (! CTFontManagerRegisterGraphicsFont(font, &error)) {
        CFStringRef errorDescription = CFErrorCopyDescription(error);
        NSLog(@"Failed to load font: %@", errorDescription);
        CFRelease(errorDescription);
    }
    CFRelease(font);
    CFRelease(provider);
    
    // eg. set the font
    self.myFontLabel.font = [UIFont fontWithName:@"Journal" size:20];
}
```


## Loading Encrypted Font

To load encrypted font, use `CGDataProviderCreateWithCFData` with the decrypted font data:

```objc
NSData *decryptedFontData = ...;
CFErrorRef error;
CGDataProviderRef provider = CGDataProviderCreateWithCFData((CFDataRef)decryptedFontData);
CGFontRef font = CGFontCreateWithDataProvider(provider);
// ... same code as above ...
```
