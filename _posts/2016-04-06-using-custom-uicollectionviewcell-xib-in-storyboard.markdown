---
layout: post
title: "Using Custom UICollectionViewCell Xib in Storyboard"
date: 2016-04-06T11:46:17+08:00
categories: [iOS]
---

There are many methods to creating your user interface - storyboard, xib, pure code, with custom cell, using view tags..

There is no right method. Different methods are for different situations. 

This post will briefly describe the different methods, with a focus on using custom `UICollectionViewCell` in a xib, which will be used in a view controller in a storyboard.


## 1. Using view tags

As always, we have a `configureCell:` method in `collectionView:cellForItemAtIndexPath:` for configuring the cell. 

```swift
func collectionView(collectionView: UICollectionView, cellForItemAtIndexPath indexPath: NSIndexPath) -> UICollectionViewCell {
    let cell = collectionView.dequeueReusableCellWithReuseIdentifier("cell", forIndexPath: indexPath)
    configureCell(cell, atIndexPath: indexPath)
    return cell
}    
```

Our simple example will have just a `UILabel` in the cell, and set to a certain name.

```swift
func configureCell(cell: UICollectionViewCell) {
    let nameLabel = cell.viewWithTag(14) as! UILabel
    nameLabel.text = ...
}
```

Using view tags is the quickest way to develop. All you need is to set a unique tag for each view in the cell in the storyboard.


## 2. Using Custom cell

Creating a custom cell is the most popular way, because you separate the view code nicely.

```swift
class MyCell: UICollectionViewCell {
    
    @IBOutlet weak var nameLabel: UILabel!
    
    var name: String {
        didSet {
            nameLabel.text = name
        }
    }
}
```

In your storyboard, you will set the cell class to `MyCell`, and connect a `UILabel` in the cell to the IBOutlet.

Then in `configureCell:`, you will instead now use the subclass `MyCell` instead of `UICollectionViewCell`.

```swift
func configureCell(cell: MyCell) {
    cell.name = ...
}
```

Note that you set the `name` property, instead of the views directly. This is a good separation of view and model code.


## 3. Using Custom cell in a xib

Using an xib instead of constructing the cell in a storyboard is useful if you want to **reuse the cell in multiple scenes**. 

This avoids duplicating the cells in different scenes/storyboards.

To do that, create `MyCell.xib`, and create the views in the xib instead. The important step here is to delete the root view that is created automatically, and add a `UICollectionViewCell`.

Then set the class name to `MyCell`, add your views and connect the IBOutlet. This step is very similar to how you do it in a storyboard in (2), except that you do it in the xib.

In the storyboard, the collection view need not have cell. 

Instead, in the view controller, this is how you set it up:

```swift
override func viewDidLoad() {
    collectionView.registerNib(UINib(nibName: "MyCell", bundle: nil), forCellWithReuseIdentifier: "cell")
}
```

You must register the cell xib so that the collection view knows about the cell.

The `configureCell:` is the same as in (2).


