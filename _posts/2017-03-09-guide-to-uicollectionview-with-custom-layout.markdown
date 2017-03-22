---
layout: post
title: "Guide to UICollectionView With Custom Layout"
date: 2017-03-09T11:10:24+08:00
categories: []
---


## What are supplementary views?

There are 3 kinds of views in a collection view. You already know about the collection view cell.

Next to know is supplementary views.

They are the accompanying views for each section.

Note that I said views, with a plural. You can have multiple supplementary views for each section. In a flow layout, you have header and footer -- that's 2 supplementary views. 

In your custom layout, you can have as many different views you want. They are distinguished by `kind` (a string). The layout object controls how many `kind` of views there are. Data source populate it after dequeuing.

You must return a resuable view in the data source method `collectionView(_:viewForSupplementaryElementOfKind:at:)`. 

If you do now want a supplementary view in a particular case, then your custom layout should not create the attribute for it. Another simpler way is to set the attribute to hide.


## And there is decoration view

The 3rd view in a collection view is a decoration view.

This is a view that is purely for decoration, and is managed by the layout object -- they do not get content from data source.


## What about autolayout?

You don't use autolayout for layout object.

It is "manual layout", that is to calculate the frames, like good old days.

But you still can use autolayout for the cells, supplementary and decoration views.




https://developer.apple.com/reference/uikit/uicollectionviewdatasource/1618037-collectionview

https://www.raywenderlich.com/107439/uicollectionview-custom-layout-tutorial-pinterest

https://www.objc.io/issues/3-views/collection-view-layouts/