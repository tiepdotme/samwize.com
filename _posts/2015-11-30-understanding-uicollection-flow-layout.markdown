---
layout: post
title: "Understanding UICollection Flow Layout"
date: 2015-11-30T17:58:53+08:00
categories: [iOS]
---

`UICollectionView` is a very powerful UI component because you can [use a flow layout](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/CollectionViewPGforIOS/UsingtheFlowLayout/UsingtheFlowLayout.html), which is kind of a dynamic grid, which provides more than a table view.

A [flow layout](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionViewFlowLayout_class/index.html) is actually a subclass of [layout](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionViewLayout_class/index.html). A (generic) layout is more powerful as you can place items in any custom layout you want! Circle layout? No problem!

But in this post, let's just restrict to the vertical flow layout.

There are two ways to implement:

1. Customize the flow layout object (simple way)
2. Implement the delegate (advanced way)


## How items are being layout

Before we look at how to implement with either the simple or advanced way, let's understand how items are layout. 

We use a verticle flow as an example (horizontal will be similar).

{%img https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/CollectionViewPGforIOS/Art/flow_horiz_layout_uneven_2x.png %}

- The tallest item in the line, will be the line height
- All items in the line will be aligned vertically center
- Minimum spacing is _minimum_ between the items, but the actual spacing depends on the collection view width
- Flow layout object will add items with minimum spacing until it can't fit, then increase the actual spacing so that they are evenly spaced
- Each section have its own line/item spacing
- In a section, the line/item spacing is fixed; you can't have different line/item spacing in a section
- Each section have its own inset


## 1. The Simple Way 

If your items have a fixed size, then you can simply use the layout object -- [`UICollectionViewFlowLayout`](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionViewFlowLayout_class/index.html#//apple_ref/occ/instp/UICollectionViewFlowLayout/)

This is an example with items that are 100x100, _at least_ 8pt apart, and with section inset of 8pt.

```swift
override func viewDidLayoutSubviews() {
    super.viewDidLayoutSubviews()

    let layout = UICollectionViewFlowLayout()
    layout.itemSize = CGSize(width: 100, height: 100)
    layout.minimumInteritemSpacing = 8
    layout.minimumLineSpacing = 8
    layout.headerReferenceSize = CGSize(width: 0, height: 40)
    layout.sectionInset = UIEdgeInsets(top: 8, left: 8, bottom: 8, right: 8)
    collectionView.collectionViewLayout = layout
}
```

Just that simple, if your items are simple.


## 2. The Advanced Way

The advanced way is required if you have varying item sizes, in which you can implement the [`UICollectionViewDelegateFlowLayout`](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionViewDelegateFlowLayout_protocol/#//apple_ref/occ/intfm/UICollectionViewDelegateFlowLayout/).

This uses the same `UICollectionViewFlowLayout` object, but you implement it's delegate methods so as to customize more advanced stuff.

For example, if each item has a different size, you will implement the following:

```swift
func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAtIndexPath indexPath: NSIndexPath) -> CGSize {
      // Return your item size
  }
```

There is a corresponding delegate method for minimum line spacing, item spacing, etc.

They are all optional, so if you don't implement, it will use the flow layout object properties.


# Bonus: How to make cells with fixed spacing?

A [common problem](http://stackoverflow.com/q/17229350/242682) is to have a **fixed spacing** between the cells.

However, you can only set `minimumInteritemSpacing`, and the actual item spacing depends on the collection view's width.

`UICollectionViewFlowLayout` will align the items center after applying the section inset, with the same amount of spacing between each cell.

If you want fixed spacing, this is [how](http://stackoverflow.com/a/34012726/242682) you can achieve by manipulating the section left & right inset:

```swift
private let minItemSpacing: CGFloat = 8
private let itemWidth: CGFloat      = 100
private let headerHeight: CGFloat   = 32

override func viewDidLayoutSubviews() {
    super.viewDidLayoutSubviews()

    // Create our custom flow layout that evenly space out the items, and have them in the center
    let layout = UICollectionViewFlowLayout()
    layout.itemSize = CGSize(width: itemWidth, height: itemWidth)
    layout.minimumInteritemSpacing = minItemSpacing
    layout.minimumLineSpacing = minItemSpacing
    layout.headerReferenceSize = CGSize(width: 0, height: headerHeight)

    // Find n, where n is the number of item that can fit into the collection view
    var n: CGFloat = 1
    let containerWidth = collectionView.bounds.width
    while true {
        let nextN = n + 1
        let totalWidth = (nextN*itemWidth) + (nextN-1)*minItemSpacing
        if totalWidth > containerWidth {
            break
        } else {
            n = nextN
        }
    }

    // Calculate the section inset for left and right. 
    // Setting this section inset will manipulate the items such that they will all be aligned horizontally center.
    let inset = min(minItemSpacing, floor( (containerWidth - (n*itemWidth) - (n-1)*minItemSpacing) / 2 ) )
    layout.sectionInset = UIEdgeInsets(top: minItemSpacing, left: inset, bottom: minItemSpacing, right: inset)

    collectionView.collectionViewLayout = layout
}
```



# Bonus: Custom Layouts

Flow layout is provided out-of-the-box. It is easy to use, and is enough for most UI.

But you can also create your own [custom layouts](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/CollectionViewPGforIOS/CreatingCustomLayouts/CreatingCustomLayouts.html).

The central method is in `layoutAttributesForElementsInRect:` of the layout class. You can read a good guide [from objc.io](https://www.objc.io/issues/3-views/collection-view-layouts/). It is an even more advanced topic.

NOTE: Usually we use autolayout constraints, but for the cell, you have to set the frame the traditional way. Just the cell. The views inside the cell may still use autolayout.

