---
layout: post
title: "Guide to NSOutlineView"
date: 2018-10-30T15:49:37+08:00
categories: [macOS]
published: false
---

## Make the row editable

You must do exactly 2 things:

1. Implement `NSOutlineViewDelegate` method `outlineView(_:shouldEdit:item)` to return true
2. Set `NSTextFieldCell` > Behaviour > Editable

It is often missed out for (2).

To make a row NOT editable, you must do both too.

## PITFALL: Model MUST be class, not struct/enum

https://forums.developer.apple.com/thread/68477

Because the _Any_ item is used throughout `NSOutlineView`, and methods such as programmatically expanding or collapsing need the item **passed by reference**.

Yea. This is pre-Swift era, where the lossy type _Any_ is being passed around. Legacy sucks.

## PITFALL: Lifecycle

There is a note in the [Apple documentation](https://developer.apple.com/documentation/appkit/nsoutlineview):

> It is possible that your data source methods for populating the outline view may be called before awakeFromNib() if the data source is specified in Interface Builder. You should defend against this by having the data source’s outlineView(_:numberOfChildrenOfItem:) method return 0 for the number of items when the data source has not yet been configured. In awakeFromNib(), when the data source is initialized you should always call reloadData().

This is abnormal, not how other views are designed eg. `UITableView`. I think it is legacy again.

If you **set the datasource in Interface Builder**, then make sure to return 0, when you have not configure your items.

Another way is to hijack in the following init, which is earlier than outlineView initialisation.

```swift
required init?(coder: NSCoder) {
    super.init(coder: coder)
    setupYourItems()
}
```

## PITFALL: Cannot collapse an item

`collapseItem` not working, but `expandItem` is okay?

This is [because](https://stackoverflow.com/a/17182276/242682) you must return true for that item in the delegate method `outlineView(_:shouldShowOutlineCellForItem:)`.

```swift
func outlineView(_ outlineView: NSOutlineView, shouldShowOutlineCellForItem item: Any) -> Bool {
    return item is GroupItem
}
```

## Show/Hide button instead of Disclosure triangle

If you have a group item, there are 2 styles:

1. Disclosure triangle on the left
2. Show/Hide label button on the right

If you want (2), you MUST change the style of the outline view to Source List. Specifically, it is `NSTableView.SelectionHighlightStyle.sourceList`.

And implement delegate method `outlineView(_:isGroupItem)` to return true.

The tradeoff is that with source list style, there is a standardised system appearance applied to the cells and text fields.

But there is a way.

You have to disconnect the `NSTextField` outlet from the cell. Then configure the text field by getting it back with tag.

```swift
func outlineView(_ outlineView: NSOutlineView, viewFor tableColumn: NSTableColumn?, item: Any) -> NSView? {
    ...
    let textField = cell.viewWithTag(0) as! NSTextField
    textField.stringValue = "Some name"

    // If you are OK with the standardised style, then you can get the text field directly from the cell
    // cell.textField?.stringValue = "Some name"
}
```
