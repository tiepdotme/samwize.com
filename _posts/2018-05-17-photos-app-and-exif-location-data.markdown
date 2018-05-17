---
layout: post
title: "Photos App & EXIF Location Data"
date: 2018-05-17T11:55:05+08:00
categories: []
---

Location, or any metadata, can be kept in 2 places:

1. EXIF metadata within the jpeg/dng/mov files
2. In Photos app database on iOS or macOS



[PHAssetCreationRequest](https://developer.apple.com/documentation/photos/phassetcreationrequest) is easy to use, and setting metadata like location is easy.

```swift
```

https://developer.apple.com/documentation/avfoundation/avcapturephotooutput/1778643-dngphotodatarepresentationforraw?language=objc

https://developer.apple.com/documentation/avfoundation/avcapturephoto/2873919-filedatarepresentation?language=objc



https://stackoverflow.com/questions/1238838/uiimagepickercontroller-and-extracting-exif-data-from-existing-photos
https://stackoverflow.com/questions/31888319/swift-accessing-exif-dictionary
https://stackoverflow.com/questions/9766394/get-exif-data-from-uiimage-uiimagepickercontroller/9890442#9890442
https://stackoverflow.com/questions/5125323/problem-setting-exif-data-for-an-image/5294574#5294574

https://stackoverflow.com/a/43376828/242682
https://stackoverflow.com/a/5294574/242682


https://gist.github.com/kwylez/a4b6ec261e52970e1fa5dd4ccfe8898f


[Modifying Image Metadata Without Recompressing Image (QA1895)](https://developer.apple.com/library/content/qa/qa1895/_index.html)


[Accessing Image Metadata in iOS (QA1622)](https://developer.apple.com/library/content/qa/qa1622/_index.html) - Using ALAssets (deprecated)
