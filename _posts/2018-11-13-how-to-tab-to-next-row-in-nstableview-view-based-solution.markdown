---
layout: post
title: "How to Tab to Next Row in NSTableView (View-based Solution)"
date: 2018-11-13T15:41:18+08:00
categories: [macOS]
---

`NSTableView` has a lot of legacy, and a simple UX of navigating from row to row (or cell to cell) proved to be very challenging.

There are solutions, but [they](http://www.knowstack.com/nstableview-tab-return/) [are](https://stackoverflow.com/a/5601129/242682) mostly for cell-based `NSTableView`, which is the legacy way.

It is possible with the modern view-based way, but there are numerous pitfalls.

## The solution

The first step is to set the delegate for the textField of the cell. You do this in your usual `NSTableViewDelegate`.

```swift
extension MyTableView: NSTableViewDelegate {
    func tableView(_ tableView: NSTableView, viewFor tableColumn: NSTableColumn?, row: Int) -> NSView? {
        ...
        cell.textField?.delegate = self
    }
}
```

Then you extend `NSTextFieldDelegate`, and implement `controlTextDidEndEditing(_:)`.

```swift
extension MyTableView: NSTextFieldDelegate {
    func controlTextDidEndEditing(_ obj: Notification) {
        guard
            let view = obj.object as? NSView,
            let textMovementInt = obj.userInfo?["NSTextMovement"] as? Int,
            let textMovement = NSTextMovement(rawValue: textMovementInt) else { return }

        let columnIndex = column(for: view)
        let rowIndex = row(for: view)

        let newRowIndex: Int
        switch textMovement {
        case .tab:
            newRowIndex = rowIndex + 1
            if newRowIndex >= numberOfRows { return }
        case .backtab:
            newRowIndex = rowIndex - 1
            if newRowIndex < 0 { return }
        default: return
        }

        DispatchQueue.main.async {
            self.editColumn(columnIndex, row: newRowIndex, with: nil, select: true)
        }
    }
}
```

This is where it is tricky for 2 reasons:

1. If you think you should be implementing in `textShouldEndEditing(_:)`, sorry somehow it is not. Thanks to [this post](http://www.taffysoft.com/pages/20171226-01.html) for pointing it out.
2. `editColumn` has to be called in `DispatchQueue` (even though it is calling from main thread.. perhaps a slight delay is needed)

This is still a side effect with the solution. When user press tab, if the table view has a next key view, that will be focused for a moment.

## Bonus: Make the table view the first responder

If you want to improve further and make the first row editable, there is some more work to do.

First, your view controller has to make the `tableView` the first responder.

```swift
view.window?.makeFirstResponder(tableView)
```

Then you need to subclass the `NSTableView`, because you need to change the behaviour when it becomes the first responder.

```swift
override func becomeFirstResponder() -> Bool {
    let index = max(editedRow, 0) // editedRow is -1 if not editing
    editColumn(1, row: index, with: nil, select: true)
    return true
}
```
