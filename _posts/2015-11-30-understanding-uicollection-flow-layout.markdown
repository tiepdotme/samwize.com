---
layout: post
title: "Understanding UICollection Flow Layout"
date: 2015-11-30T17:58:53+08:00
categories: [iOS]
---

[Using Flow Layout](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/CollectionViewPGforIOS/UsingtheFlowLayout/UsingtheFlowLayout.html)

Two ways to implement:

1. Customize the attribute (simple way)
2. Implement the delegate (advanced way)


## How items are layout

Before we look at how to implement with either the simple or advanced way, let's understand how items will be layout. We use a verticle scrolling flow (horizontal will be similar).

- The tallest item in the line, will be the line height
- All items in the line will be aligned vertically center
- Minimum spacing is _minimum_ between the items, but the actual spacing depends on the collection view width
- Flow layout object will add items with minimum spacing until it can't fit, then increase the actual spacing so that they are evenly spaced
- Each section can have its own line/item spacing
- In a section, the line/item spacing is fixed; you can't have different line/item spacing in a section


## Simple way 

https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionViewFlowLayout_class/index.html#//apple_ref/occ/instp/UICollectionViewFlowLayout/

`minimumLineSpacing`

`minimumInteritemSpacing`

`itemSize`

`sectionInset`



## Advanced way

https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionViewDelegateFlowLayout_protocol/#//apple_ref/occ/intfm/UICollectionViewDelegateFlowLayout/

The advanced way will be needed eg. each item has different size.

collectionView:layout:minimumLineSpacingForSectionAtIndex:

collectionView:layout:minimumInteritemSpacingForSectionAtIndex: 


## Section Insets

Insets applies to a section.

You could use it to control how many items on each line.


## Custom Layouts

Flow layout is provided out-of-the-box.

You can create your own [custom layouts](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/CollectionViewPGforIOS/CreatingCustomLayouts/CreatingCustomLayouts.html#//apple_ref/doc/uid/TP40012334-CH5-SW21).
