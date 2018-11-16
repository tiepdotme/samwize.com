---
layout: post
title: "Guide to NSFetchedResultsController With NSTableView (macOS)"
date: 2018-11-16T10:20:44+08:00
categories: [macOS, osx]
---

I have a post on implementing `NSFetchedResultsController` [for iOS `UITableView`](/2015/10/27/implementing-nsfetchedresultscontroller-in-swift/).

This time, let's take a look at how to implement the same but for the Cocoa framework counterpart - **`NSTableView` for macOS**.

## The Differences

The table view APIs are quite different, since the UI representation is pretty different.

- iOS has concept of sections * rows
  - macOS has only rows
- iOS concept of 1 cell = 1 row
  - macOS's 1 row = multiple cells, aka columns

In other words, iOS has sections and no columns, while macOS has columns and no sections.

## 1. Setup NSFetchedResultsController

As usual, create the object as a lazy var.

```swift
private lazy var fetchedResultsController: NSFetchedResultsController<Networth> = {
    let fetchRequest: NSFetchRequest<Networth> = Networth.fetchRequest()
    fetchRequest.predicate = predicate ...
    fetchRequest.sortDescriptors = ...
    let frc = NSFetchedResultsController(fetchRequest: fetchRequest, managedObjectContext: container.viewContext, sectionNameKeyPath: nil, cacheName: nil)
    frc.delegate = self
    return frc
}()
```

In your view controller's `viewDidLoad`, call `try fetchedResultsController.performFetch()`.

You might also be interested to read my post on [how to use Core Data](/2018/09/01/modern-guide-to-core-data-2018/), including the new container's view context.

## 2. Setup table view delegates

`NSTableView` has far fewer methods that needs to be implemented. Just 1 for `NSTableViewDataSource` and 1 for `NSTableViewDelegate`.

```swift
// NSTableViewDataSource:

func numberOfRows(in tableView: NSTableView) -> Int {
    return fetchedResultsController.fetchedObjects?.count ?? 0
}

// NSTableViewDelegate:

func tableView(_ tableView: NSTableView, viewFor tableColumn: NSTableColumn?, row: Int) -> NSView? {
    let cell: NSTableCellView!
    let column = tableView.tableColumns.firstIndex(of: tableColumn!)!
    switch column {
    case 0:
        cell = tableView.makeView(withIdentifier: NSUserInterfaceItemIdentifier(rawValue: "date"), owner: nil) as? NSTableCellView
    case 1:
        cell = tableView.makeView(withIdentifier: NSUserInterfaceItemIdentifier(rawValue: "amount"), owner: nil) as? NSTableCellView
    default:
        return nil
    }

    configureCell(cell: cell, row: row, column: column)
    return cell
}

private func configureCell(cell: NSTableCellView, row: Int, column: Int) {
    let networth = fetchedResultsController.fetchedObjects![row]
    switch column {
    case 0:
        cell.textField?.stringValue = networth.date ?? ""
    case 1:
        cell.textField?.stringValue = networth.amount ?? ""
    default:
        break
    }
}
```

`configureCell` is an abstraction because we need to use it later. Customize as needed for your app, and for all the columns you have.

## 3. NSFetchedResultsControllerDelegate

The methods in `NSFetchedResultsControllerDelegate` is again a chunk of code that you can simply copy and paste, without any modification:

```swift
// NSFetchedResultsControllerDelegate:

func controller(_ controller: NSFetchedResultsController<NSFetchRequestResult>, didChange anObject: Any, at indexPath: IndexPath?, for type: NSFetchedResultsChangeType, newIndexPath: IndexPath?){
    switch type {
    case .insert:
        if let newIndexPath = newIndexPath {
            tableView.insertRows(at: [newIndexPath.item], withAnimation: .effectFade)
        }
    case .delete:
        if let indexPath = indexPath {
            tableView.removeRows(at: [indexPath.item], withAnimation: .effectFade)
        }
    case .update:
        if let indexPath = indexPath {
            let row = indexPath.item
            for column in 0..<tableView.numberOfColumns {
                if let cell = tableView.view(atColumn: column, row: row, makeIfNecessary: true) as? NSTableCellView {
                    configureCell(cell: cell, row: row, column: column)
                }
            }
        }
    case .move:
        if let indexPath = indexPath, let newIndexPath = newIndexPath {
            tableView.removeRows(at: [indexPath.item], withAnimation: .effectFade)
            tableView.insertRows(at: [newIndexPath.item], withAnimation: .effectFade)
        }
    }
}

func controllerWillChangeContent(_ controller: NSFetchedResultsController<NSFetchRequestResult>) {
    tableView.beginUpdates()
}

func controllerDidChangeContent(_ controller: NSFetchedResultsController<NSFetchRequestResult>) {
    tableView.endUpdates()
}
```

What's new is that in `.update`, the table view needs to configure for all the columns for that row.
