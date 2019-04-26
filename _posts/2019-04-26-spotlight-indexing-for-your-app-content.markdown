---
layout: post
title: "Spotlight Indexing for Your App Content"
date: 2019-04-26T15:49:43+08:00
categories: [iOS]
---

If your app has any data or content, you will probably want to implement spotlight indexing, so that users can conveniently search in spotlight.

![A searched item](https://developer.apple.com/library/archive/documentation/General/Conceptual/AppSearch/Art/numbers_2x.png)

## 1. Create the searchable items

Let's start with creating `CSSearchableItem` for a model in the app. We use the model `station` as an example.

```swift
import CoreSpotlight
import MobileCoreServices

func createSearchableItem(_ station: Station) -> CSSearchableItem {
    let attributeSet = CSSearchableItemAttributeSet(itemContentType: kUTTypeData as String)
    attributeSet.title = station.name
    attributeSet.contentDescription = station.description
    attributeSet.thumbnailData = station.logo.pngData()
    return CSSearchableItem(uniqueIdentifier: station.id, domainIdentifier: "station", attributeSet: attributeSet)
}
```

The above is the minimal data that you are recommended to provide. You could provide more [metadata](https://developer.apple.com/documentation/corespotlight/cssearchableitemattributeset).

## 2. Index them all

```swift
func indexAll() {
    let stations = ...
    let items = stations.map { createSearchableItem($0) }
    CSSearchableIndex.default().indexSearchableItems(items) {
        if let error = $0 {
            logError(error.localizedDescription)
        }
    }
}
```

Using `CSSearchableIndex`, you add your items to the index. Do a search, and you should see in spotlight results.

_PITFALL: Simulator might not work, so try on a real device._

## 3. Handle when tap on an item

In your app delegate, implement `application:continueUserActivity:restorationHandler:`, the user activity handoff method.

```swift
func application(_ application: UIApplication, continue userActivity: NSUserActivity, restorationHandler: @escaping ([UIUserActivityRestoring]?) -> Void) -> Bool {
    if userActivity.activityType == CSSearchableItemActionType {
        if let identifier = userActivity.userInfo? [CSSearchableItemActivityIdentifier] as? String {
            // Handle it. Navigate to a certain screen, or perform certain action.
            return true
        }
    }
    return false
}
```

The same app delegate method is [used for universal link](https://samwize.com/2017/11/10/guide-to-universal-links/) too.

## 4. Maintain the index

To update an item, use `indexSearchableItems()` with the same `uniqueIdentifier`.

To delete, you can use `deleteSearchableItemsWithIdentifiers` and [other methods](https://developer.apple.com/documentation/corespotlight/cssearchableindex).
