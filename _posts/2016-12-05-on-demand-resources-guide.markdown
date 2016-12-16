---
layout: post
title: "On-Demand Resources Guide"
date: 2016-12-05T10:23:11+08:00
categories: [iOS]
---

[On-Demand Resources](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/Tagging.html) is a new developer feature provided in iOS 8.

It is lazy loading of resources, so that app binary size can be reduced.

In other words, when you require a resource, you call the API to download it.


## Tagging

To use, "tag" your resources (any files or assets).

You can assign multiple tags per resource.

Tagging is a way of grouping resources in a common bucket. When you need to use these individual resource, you have to download the tag (which will download ALL the resources, not individually).

So if you want to download only 1 resource, then have 1 unique tag for that resource.


## 3 Types of Resource Tag

1. **Initial install tags** 
  - Downloaded at the same time as the app
  - Included in the total size for the app
  - The only benefit -- the tags can be purged after use

2. **Prefetch tag order**
  - Downloaded right after the app is installed, in the order listed
  - Excluded in the total size for the app

3. **Dowloaded only on demand** (DEFAULT)
  - Downloaded when requested by the app
  - Excluded in the total size for the app

You can change the type by going to **Target > Resource Tags > Prefetched**.


## Download the Tags

Use [NSBundleResourceRequest](https://developer.apple.com/reference/foundation/nsbundleresourcerequest) to specify the tag(s) to download.

Init `NSBundleResourceRequest` with `initWithTags:`, which will download and put the resources in the main bundle. 

The main API to use is `beginAccessingResourcesWithCompletionHandler:`. This will start to download the resources, if they are not already in the device. 

If the resources are already in the device, then it will just call the completion block.

Another method is `conditionallyBeginAccessingResourcesWithCompletionHandler:`. This merely check if the resources are already on device (it will not download even if it is not present). I don't see much use of calling this asynchronous method just to check.

Note: The completion block is not called on the main thread. 


## Using the Resource

You can use the main bundle, after the tags are downloaded.

```swift
NSBundle.mainBundle().URLForResource("myvideo", withExtension: "mp4")!
```

For `UIImage`, as usual, you can use `imageNamed`, and it will be there.

But remember, it is up to you to make sure the resources/tags are downloaded. You must ensure they are successfully downloaded before use.

If the resource is not on device, it could crash. 


## Pitfall: Bugs during Development

However, things will [not work](http://stackoverflow.com/q/39870159/242682) (tested in Xcode 8.1).

Tags with **Initial install tags** are not downloaded after app is installed on simulator. You can see it is not downloaded under **Debug navigator > Disk**.

This is obviously a bug.

There is no workaround, other than to always ensure your resources are downloaded (which you definitely should) before use.


## Preservation Priority

The operating system could purge any resource that is not held by any `NSBundleResourceRequest`.

Purging starts with resources with lowest preservation priority, between 0 and 1.

Use [setPreservationPriority(_:forTags:)](https://developer.apple.com/reference/foundation/bundle/1614845-setpreservationpriority) on the bundle.

It is still not clear when the OS will purge. But it could persist across multiple launches. 