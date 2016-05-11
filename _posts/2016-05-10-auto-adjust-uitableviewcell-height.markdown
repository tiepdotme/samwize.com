---
layout: post
title: "Auto Adjust UITableViewCell Height"
date: 2016-05-10T16:50:59+08:00
categories: [iOS]
---

If you use auto layout and support iOS 8, then you are in luck.

In the past, you will need to implement `tableView:heightForRowAtIndexPath:` and caculate the height of your cell to return. It is a lot of code just to achieve the goal.

Now, it is just these [steps](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/WorkingwithSelf-SizingTableViewCells.html):

## Step 1. Set `rowHeight`

```swift
tableView.rowHeight = UITableViewAutomaticDimension
// Similarly, you can set for header & footer
tableView.sectionFooterHeight = UITableViewAutomaticDimension
```

`UITableViewAutomaticDimension` is a special value (a `CGFloat`) that tells the cell to auto adjust it's height.

You may even return it in `tableView:heightForRowAtIndexPath:`, if you want a mix of auto and fixed height.


## Step 2. Set `estimatedRowHeight`

```swift
tableView.estimatedRowHeight = 85.0
```

You will also need to set the `estimatedRowHeight`. 

If not, it will NOT work.

The estimated height is needed only for the purpose of calculating the scroll bar height.


## Step 3. Autolayout for the cells

Set up autolayout constraints as per normal. 

Make sure that the content view has the necessary contraints eg. the bottom is pinned to some view


