---
layout: post
title: "Resize UIImage in Swift"
date: 2016-06-01T14:28:06+08:00
categories: [iOS, Swift]
---

Erica Sadun posted a [Swift rewrite challenge](http://ericasadun.com/2016/05/31/swift-rewrite-challenge/), and the [chosen function](https://gist.github.com/jkereako/200342b66b5416fd715a) is to scale and crop an image.

Resizing a `UIImage` is a [very common task](http://stackoverflow.com/q/2658738/242682).

I have written it a couple of times, in Objective-C, and recently in Swift.

While I am not partaking in the challenge, it is helpful to look at the different implementations, learn from [them](https://gist.github.com/erica/157e20ea0c7e9f28a03a8b12448c8fd0), and improve my version.

## My Improved Code

```Swift
import UIKit

extension UIImage {
    
    /// Returns a image that fills in newSize
    func resizedImage(newSize: CGSize) -> UIImage {
        // Guard newSize is different
        guard self.size != newSize else { return self }
        
        UIGraphicsBeginImageContextWithOptions(newSize, false, 0.0);
        self.drawInRect(CGRectMake(0, 0, newSize.width, newSize.height))
        let newImage: UIImage = UIGraphicsGetImageFromCurrentImageContext()
        UIGraphicsEndImageContext()
        return newImage
    }
    
    /// Returns a resized image that fits in rectSize, keeping it's aspect ratio
    /// Note that the new image size is not rectSize, but within it.
    func resizedImageWithinRect(rectSize: CGSize) -> UIImage {
        let widthFactor = size.width / rectSize.width
        let heightFactor = size.height / rectSize.height
        
        var resizeFactor = widthFactor
        if size.height > size.width {
            resizeFactor = heightFactor
        }
        
        let newSize = CGSizeMake(size.width/resizeFactor, size.height/resizeFactor)
        let resized = resizedImage(newSize)
        return resized
    }
    
}

// Tests
guard let url = NSURL(string: "http://placehold.it/300x150") else { fatalError("Bad URL") }
guard let data = NSData(contentsOfURL: url) else { fatalError("Bad data") }
guard let img = UIImage(data: data) else { fatalError("Bad data") }

let outImageFit = img.resizedImageWithinRect(CGSize(width: 200, height: 200))
let outImageFill = img.resizedImage(CGSize(width: 200, height: 200))
```

It is my duty to point out the 0.0 passed in `UIGraphicsBeginImageContextWithOptions` is to make sure the [image scale gracefully in retina devices](/2016/04/19/pitfall-drawing-with-core-graphics-gives-blurry-lines/).