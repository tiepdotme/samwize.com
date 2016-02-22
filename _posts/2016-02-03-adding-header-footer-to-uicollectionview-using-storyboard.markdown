---
layout: post
title: "Adding Header/Footer to UICollectionView Using Storyboard"
date: 2016-02-03T15:34:44+08:00
categories: [iOS]
---

There are a few tutorials on on `UICollectionView` which you teach you to programmatically create a header/footer view as a subclass of `UICollectionReusableView`.

I love to use storyboard, so this is a tutorial to provide the steps to adding a header view (footer is similar) to a `UICollectionView`.

## 1. Add Header to Storyboard

Select Collection View > Attributes Inspector > Enabled **Section Header**.

Once that is enabled, a section view will appear, and you can drag your views to it.

Select the header view, and set the **Identifier** (will be used next).


## 2. Implement UICollectionViewDataSource method `viewForSupplementaryElementOfKind`

```swift
func collectionView(collectionView: UICollectionView, viewForSupplementaryElementOfKind kind: String, atIndexPath indexPath: NSIndexPath) -> UICollectionReusableView {
    let headerView = collectionView.dequeueReusableSupplementaryViewOfKind(kind, withReuseIdentifier: headerId, forIndexPath: indexPath)
    return headerView
}
```

In the above method, `headerId` is the header view identifier you set in storyboard.


## 3. Set layout header size

Lastly, and this is most often left out by developers, is that you still need to set the header view size in the layout.

Most would have thought that the header view size is that in the storyboard, but sadly, it is not (and the default size is 0)!

```swift
layout.headerReferenceSize = CGSizeMake(0, 50);
```

`layout` is the custom layout such as a `UICollectionViewFlowLayout`, which you set to `collectionView.collectionViewLayout`.
