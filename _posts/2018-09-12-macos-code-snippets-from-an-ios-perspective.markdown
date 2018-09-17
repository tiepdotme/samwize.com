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

Remove in your target's **Deployment Info > Main Interface** ([screenshot](/2018/04/04/setup-appdelegate-without-storyboard/)) and programmatically show your view controller in AppDelegate. However, I _could not programmatically_ get this to work.

Alternatively don't use `applicationDidFinishLaunching` to setup your stack. Use your subclassed `NSWindowController` init (1 of the init depending if you create by code or nib).

```swift
class MainWindowController: NSWindowController {
    required init?(coder: NSCoder) {
        super.init(coder: coder)
        // Setup stack here. An example of database setup:
        DB.default.setup(dataModelName: "MyDatabase")
}
```

## Pitfall: Cocoa Binding not key value compliant

> Terminating app due to uncaught exception 'NSUnknownKeyException', reason: '[<MyApp.MainViewController 0x600000120a00> valueForUndefinedKey:]: this class is not key value coding-compliant for the key context.'

If you use binding, ensure that the property is annotated with `@objc dynamic`, because cocoa binding uses dynamic properties. For example, if you bind a `NSManagedObjectContext`, it must look like this in the controller:

```swift
@objc dynamic var context: NSManagedObjectContext!
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
