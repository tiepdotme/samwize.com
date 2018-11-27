---
layout: post
title: "Drag and Drop to Reorder NSTableView"
date: 2018-11-27T09:53:52+08:00
categories: [macOS]
---

To support reordering of rows in a `NSTableView`, you need to understand how a table works with drag and drop operation.

In a drag and drop operation, there is a **source** and a **destination**. They can be the same table view, or can be different views (any kind of view)! In the example code below, we use the same table view, but I have added comments on which is for source/destination.

There are 3 methods of `NSTableViewDataSource` to implement.

I will use an example of a table view of accounts, with an array of `accounts` as the data model. This data model could be a `fetchedResultsController.fetchedObjects` for Core Data. To persist the new ordering, you have to save it yourself.

## 1. Write to pasteboard when dragged

The first method is for the source table view. When a row is being dragged, you write your model to pasteboard.

The pasteboard is like a temporary holding area for the dragged item.

```swift
extension AccountsView: NSTableViewDataSource {
    // For the source table view
    func tableView(_ tableView: NSTableView, pasteboardWriterForRow row: Int) -> NSPasteboardWriting? {
        let account = accounts![row]
        let pasteboardItem = NSPasteboardItem()
        pasteboardItem.setString(account.uuid, forType: accountPasteboardType)
        return pasteboardItem
    }
}
```

The method returns a concrete `NSPasteboardItem`, which implements the protocol `NSPasteboardWriting`. The right way is to implement the protocol for your model and return the model.

We use `account.uuid`, which is a String representation. If you use Core Data `NSManagedObject`, you can use `objectID.uriRepresentation().absoluteString`.


## 2. Handle when dropped

Not surprisingly, the other 2 methods are for the destination table view when dropped.

The first method is simple to return `.move`.

The second method is to handle `acceptDrop`, persist the new ordering, then animate the rows.

```swift
extension AccountsView: NSTableViewDataSource {
    // For the destination table view
    func tableView(_ tableView: NSTableView, validateDrop info: NSDraggingInfo, proposedRow row: Int, proposedDropOperation dropOperation: NSTableView.DropOperation) -> NSDragOperation {
        return .move
    }

    // For the destination table view
    func tableView(_ tableView: NSTableView, acceptDrop info: NSDraggingInfo, row: Int, dropOperation: NSTableView.DropOperation) -> Bool {
        guard
            let item = info.draggingPasteboard.pasteboardItems?.first,
            let theString = item.string(forType: accountPasteboardType),
            let account = accounts?.first(where: { $0.uuid == theString }),
            let originalRow = accounts?.firstIndex(of: account)
            else { return false }

        var newRow = row
        // When drag to beyond the last, the row is OOB. Prevent that by normalizing newRow.
        if dropOperation == .above && row > 0 && row >= accounts!.count {
            newRow = row - 1
        }

        // Persist the ordering by saving your data model
        saveAccountsReordred(at: originalRow, to: newRow)

        // Animate the rows
        tableView.beginUpdates()
        tableView.moveRow(at: originalRow, to: newRow)
        tableView.endUpdates()

        return true
    }
}
```

You retrieve the `NSPasteboardItem` from the `info` object, and with that string representation you retrieve the account model and the original row (the index in the data model array).

There are different `dropOperation` -- `above` or `on` a row. When you drag beyond the last row, it will be _above the (last row + 1) item_. We prevent the out-of-bound exception.

The persisting of the new order and the animation or the rows are 2 separate operations.

## Multiple items

The example we have use a **single item** for drag and drop.

If you support multiple items, it get more complicated during the `acceptDrop` phase. You can refer to [this stackoverflow answer](https://stackoverflow.com/a/52368491/242682) on how to move all of the rows.
