---
layout: post
title: "Everything About UITableView"
date: 2016-02-24T16:25:31+08:00
categories: [iOS]
published: false
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

## Make a cell unselectable

```swift
cell.selectionStyle = .None
```
