---
layout: post
title: "Photos App & EXIF Location Data"
date: 2018-05-17T11:55:05+08:00
categories: []
---

Location, or any metadata, can be saved in 2 places:

1. In Photos app's database
2. In the jpg/heic/dng/mov files

You WILL have to do both for an iOS app, because if you don't, then you will lose the information along the way eg importing.

_TIP: Use `exiftool -a -u -g1 filename.jpg` to examine the info._

## 1. Photos app's database

Let's start with the easy one.

[PHAssetCreationRequest](https://developer.apple.com/documentation/photos/phassetcreationrequest) makes it very easy to add the location data to the database.

```swift
PHPhotoLibrary.shared().performChanges({
    let creationRequest = PHAssetCreationRequest.forAsset()
    creationRequest.location = latestLocation
    creationRequest.addResource(with: .photo, data: photoData, options: options)
    ...
})
```

You can also set the `creationDate` and `isFavorite`.

## The Problem

If you do (1), but not (2), then when the photo is imported into macOS/Windows, surprisingly, even using Apple's Photo app, metadata will be lost.

When importing, only the **metadata in the file** is used.

So you need to write metadata to the file, and that is HARDER.

## 2. Writing metadata to file

If you are using AVFoundation to capture photo, then it is relatively easy. There is a [variant of `fileDataRepresentation`](https://developer.apple.com/documentation/avfoundation/avcapturephoto/2875953-filedatarepresentation) that can take in the metadata.

This is what you do in the `AVCapturePhotoCaptureDelegate` method:

```swift
func photoOutput(_ output: AVCapturePhotoOutput, didFinishProcessingPhoto photo: AVCapturePhoto, error: Error?) {
    var metadata = photo.metadata
    metadata[kCGImagePropertyGPSDictionary as String] = gpsMetadata
    photoData = photo.fileDataRepresentation(withReplacementMetadata: metadata,
      replacementEmbeddedThumbnailPhotoFormat: photo.embeddedThumbnailPhotoFormat,
      replacementEmbeddedThumbnailPixelBuffer: nil,
      replacementDepthData: photo.depthData)
    ...
}
```

With the `photoData`, you can save to file or add to Photos using `PHAssetCreationRequest`.

## Using Image I/O and other frameworks

If you are not capturing photo using AVFoundation, then you don't have the luxury of the above to get the `fileDataRepresentation` along with the metadata.

In that case, you have to add metadata manually to the original image data.

Image I/O Framework provides methods to [Modifying Image Metadata Without Recompressing Image (QA1895)](https://developer.apple.com/library/content/qa/qa1895/_index.html), using [`CGImageDestinationCopyImageSource`]( https://developer.apple.com/documentation/imageio/1465189-cgimagedestinationcopyimagesourc?language=objc), which supports JPEG, PNG, PSD, TIFF.

The `options_dict` in the sample code is the metadata dictionary.

Check out S/O answers like [this](https://stackoverflow.com/a/5294574/242682) or [this](https://stackoverflow.com/a/43376828/242682) (using Core Media).

Assets Library framework (deprecated) also has it's own methods to [Accessing Image Metadata in iOS (QA1622)](https://developer.apple.com/library/content/qa/qa1622/_index.html).

## Metadata

Wonder what kind of structure is `metadata`?

Metadata is a dictionary of dictionaries.

[CGImageProperties](https://developer.apple.com/documentation/imageio/cgimageproperties) has a reference to the dictionaries you can define. For example, [GPS has it's own dictionary keys](https://developer.apple.com/documentation/imageio/cgimageproperties/gps_dictionary_keys), so does [EXIF](https://developer.apple.com/documentation/imageio/cgimageproperties/exif_dictionary_keys).

```swift
// gpsMetadata and exifMetadata are 2 dictionaries
metadata[kCGImagePropertyGPSDictionary as String] = gpsMetadata
metadata[kCGImagePropertyExifDictionary as String] = exifMetadata
```

There is a helper on turning `CLLocation` into the [GPS data in a dictionary](https://stackoverflow.com/a/5314634/242682).

A [Swift extension](https://gist.github.com/kwylez/a4b6ec261e52970e1fa5dd4ccfe8898f) is kindly available too.
