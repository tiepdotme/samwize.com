---
layout: post
title: "The bug with UICollectionView Content Offset"
date: 2016-04-04T13:47:14+08:00
categories: [iOS]
---

I have covered a [few](http://samwize.com/2015/11/30/understanding-uicollection-flow-layout/) [posts](http://samwize.com/2016/02/03/adding-header-footer-to-uicollectionview-using-storyboard/) [on](http://samwize.com/2015/03/05/pitfall-nslayoutconstraint-breaking-in-uicollectionview/) using `UICollectionView`.

This post is about a bug to do with the content offset that somehow get set incorrectly during creation of the layout. I have no solution to this bug.


## The Cause

In iOS 7, the [behaviour changed](https://devforums.apple.com/message/874718#874718), and `viewDidLayoutSubviews` gets called every time that it loads a `UICollectionViewCell`. 

An effect of repeatedly calling `viewDidLayoutSubviews` is that the collection view content offset will be _incorrectly set to something_ during one of the calls, causing the collection view to be "scrolled" to a wrong position.


## Where to create layout?

The bug is triggered when layout is created. Exactly where the following code (lets refer to `setupLayout`) is called matters.

```swift
func setupLayout() {
    // Create your layout
    let layout = ...
    collectionView.collectionViewLayout = layout
}
```

Is in `viewDidLoad`? 

Unfortunately, in `viewDidLoad`, the **collection view bounds are not ready yet**, so depending on your layout (if it needs to know the bounds), it might not be the correct place to set up your layout.

Placing it in `viewDidLayoutSubviews` is a better choice, because the collection view is initialized and the bounds are now correct.


## The problem with `viewDidLayoutSubviews`

When you create your layout in `viewDidLayoutSubviews`, there will be the mentioned buggy behaviour whereby the collection view will somehow set it's content offset, making the view scrolled slightly (down/right).

If you trace the code, `viewDidLayoutSubviews` will be called twice or more, with one of the call changing the `contentOffset` (eg y by 38pt).

To fix, make sure you create the layout only ONCE.

```swift
var viewDidLayoutSubviewsForTheFirstTime = true

override func viewDidLayoutSubviews() {
    super.viewDidLayoutSubviews()

    // Make sure this is the first time, else return
    guard viewDidLayoutSubviewsForTheFirstTime == true else {return}
    viewDidLayoutSubviewsForTheFirstTime = false

    // Create the layout
    setupLayout()
}
```

## But what when bounds change?

Unfortunately, the above prevents layout from adjusting when the bounds change eg. device orientation rotated

This is because `viewDidLayoutSubviews` is the place to adjust a collection view layout, because the new bounds (specifically the collection view bounds) would have been updated.

So, we are back to the problem:

- setting up layout in `viewDidLayoutSubviews`
- bug with content offset

A likely solution is to adjust the content offset for the first time the layout is created.

However, that don't work, because the content offset might not be set on first time!


## Conclusion

There is no solution I know of.

But depending on your situation, it could still work.

If your layout does NOT require information of the collection view bounds:

```swift
// Set up in `viewDidLoad`
override func viewDidLoad() {
    super.viewDidLoad()
    setupLayout()
}
```

Otherwise, this code, but with the bug that the content offset will be off:

```swift
// Set up in `viewDidLayoutSubviews`
override func viewDidLayoutSubviews() {
    super.viewDidLayoutSubviews()
    setupLayout()
}
```

If you don't need to deal with bounds/orientation change, then you can put on a guard to make sure layout is created once only:

```swift
var viewDidLayoutSubviewsForTheFirstTime = true
 
override func viewDidLayoutSubviews() {
    super.viewDidLayoutSubviews()
 
    // Make sure this is the first time, else return
    guard viewDidLayoutSubviewsForTheFirstTime == true else {return}
    viewDidLayoutSubviewsForTheFirstTime = false
 
    // Create the layout
    setupLayout()
}
```