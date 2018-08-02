---
layout: post
title: "Everything About UITableView and UITableViewCell"
date: 2016-02-24T16:25:31+08:00
categories: [iOS]
---

To configure `tableView`, you will typically set it's properties in `viewDidLoad`.

To configure `cell`, you will typically set it's properties in `tableView:cellForRowAtIndexPath:`, after dequeuing it of course.

## Hide all separator lines

```swift
tableView.separatorStyle = .None
// Or
tableView.separatorColor = UIColor.clearColor()
```

This will hide all separator lines, including the top and bottom which bound the section. Read next section to hide only the lines between cells.

## Hide separator lines betweeen cells

```swift
cell.separatorInset = UIEdgeInsetsMake(0, cell.bounds.size.width, 0, 0);
```

## Hide separator lines for the empty cells

Somehow, table view will have separator lines in the empty space when there ain't enough rows to fill a screen. This is a workaround.

```swift
# Set an empty footer
tableView.tableFooterView = UIView()
```

## Full width separtor line

```swift
cell.separatorInset = UIEdgeInsetsMake(0, 0, 0, 0);
cell.layoutMargins = UIEdgeInsetsMake(0, 0, 0, 0);
```

## Make a cell unselectable

```swift
cell.selectionStyle = .None
```

## Change cell selection background color

```swift
let bg = UIView()
bg.backgroundColor = UIColor.redColor()
cell.selectedBackgroundView = bg
```

## Handling rotation

When device rotate, you should reload data so that the cells will reload.

```swift
override func willRotateToInterfaceOrientation(toInterfaceOrientation: UIInterfaceOrientation, duration: NSTimeInterval) {
    tableView.reloadData()
}
```

## Using custom cell in nib

A [similar tutorial on using nib for UICollectionViewCell](/2016/04/06/using-custom-uicollectionviewcell-xib-in-storyboard/) was written. `UITable` is smiliar.

```swift
tableView.registerNib(UINib(nibName: "MyCell", bundle: nil), forCellReuseIdentifier: "cell")
```

## Highlighting cell

Cell is highlighted when a touch is held on it.

UITableView provides a default behaviour:

1. It will change the `cell.backgroundColor` to gray
2. It will change **ALL** subviews backgroundColor to clear

The result of (2) will cause your [subview to "disappear"](https://stackoverflow.com/q/6745919/242682). The solution is to add another UIView/CALayer to your custom subview as a background.

If you don't like gray, you can change the [`selectionStyle`](https://developer.apple.com/documentation/uikit/uitableviewcellselectionstyle) or `selectedBackgroundView`.

## Table Section Header/Footer

Read [this post](/2015/11/06/guide-to-customizing-uitableview-section-header-footer/) on adding header/footer.

## Table Header/Footer

This is for the table, not each section.

If you are not using autolayout, it is simply setting the `tableHeaderView` or `tableFooterView`.

It gets tricky if you are using autolayout, as iOS has not support well for it (yet)! Refer to [this post](/2017/05/25/creating-a-autoresizing-uitableview-programmatically/).
