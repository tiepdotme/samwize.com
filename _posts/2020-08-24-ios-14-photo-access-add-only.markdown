---
layout: post
title: "iOS 14 Photo Access for Add Only"
date: 2020-08-24T20:31:07+08:00
categories: [Photo]
---

iOS 14 changed the permission levels when you access photo library, and many apps will now prompt users to either:

1. Select photos
2. Allow all
3. Deny access

This happens if you use `PHPhotoLibrary.requestAuthorization(...)`.

## But what if write only?

If you are writing to photo library, without the need to read, the same prompt will be used.

That is no good.

There is a better way for write-only apps, but you have to update your code.

## 1. Use addOnly

There is a [NEW iOS 14 method](https://developer.apple.com/documentation/photokit/phphotolibrary/3616053-requestauthorization) in PHPhotoLibrary framework  with a `addOnly` option.

`PHPhotoLibrary.requestAuthorization(for: .addOnly, handler: handler)`.

If you want to support pre iOS 14, then you will want to use [platform conditional codes](/2020/07/22/platform-conditional-codes/) like this:

```swift
let handler: (PHAuthorizationStatus) -> Void = { status in
    ...
}

if #available(iOS 14, *) {
    PHPhotoLibrary.requestAuthorization(for: .addOnly, handler: handler)
} else {
    PHPhotoLibrary.requestAuthorization(handler)
}
```

## 2. Add NSPhotoLibraryAddUsageDescription

In Info.plist, add the key `NSPhotoLibraryAddUsageDescription` and a string to describe why you need the access.
