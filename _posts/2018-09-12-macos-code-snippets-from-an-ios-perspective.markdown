---
layout: post
title: "macOS Code Snippets From an iOS Perspective"
date: 2018-09-12T15:13:27+08:00
categories: [macOS]
published: false
---

I have developed on iOS for 10 years, and only now did I try to learn macOS. Wow it is so different with even more legacies stuff.

This is a compilation of the common UI tasks.

## Dismiss view controller

```swift
view.window?.close()
```

## Show an error alert

```swift
let alert = NSAlert(error: someError)
alert.runModal()
```

## Show an alert

```swift
let alert = NSAlert()
alert.style = .informational
alert.messageText = ...
alert.icon = ...
alert.addButton(withTitle: "First Button")
let result = alert.runModal()

if result == .alertFirstButtonReturn {
    // First button was clicked
}
```

## Table View Datasource

Quirk: The "data source" method is in `NSTableViewDelegate`.

```swift
extension MyViewController: NSTableViewDelegate {
    func tableView(_ tableView: NSTableView, viewFor tableColumn: NSTableColumn?, row: Int) -> NSView? {
        let column = tableView.tableColumns.firstIndex(of: tableColumn!)!
        let cell: NSTableCellView!
        switch column {
        case 0:
            cell = tableView.makeView(withIdentifier: NSUserInterfaceItemIdentifier(rawValue: "foo"), owner: nil) as? NSTableCellView
            cell.textField?.stringValue = ...
        // Other cases ...
        default:
            return nil
        }
        return cell
    }
```

You might think the data source comes from `tableView(_:setObjectValue:for:row:)`. Nope. This method is used only for binding cell based table view.

## `applicationDidFinishLaunching` fire after `viewDidLoad`

Quirk: Cocoa app will load the main storyboard/xib first, then `applicationDidFinishLaunching`.

Remove in your target's **Deployment Info > Main Interface** ([screenshot](/2018/04/04/setup-appdelegate-without-storyboard/)) and programmatically show your view controller in AppDelegate. However, I could not get this to work.

Alternatively don't use `applicationDidFinishLaunching` to setup your stuff. Use `MainViewController.init/viewDidLoad`, but do make sure only run once, then control the flow to other view controllers.

##

```swift
```

##

```swift
```

##

```swift
```

##

```swift
```

##

```swift
```
