---
layout: post
title: "Implementing NSFetchedResultsController in Swift"
date: 2015-10-27T12:10:26+08:00
categories: [iOS]
---

This is an updated post for [Implementing NSFetchedResultsController with MagicalRecord](/2014/03/29/implementing-nsfetchedresultscontroller-with-magicalrecord/), with these changes:

1. Code is in Swift
2. No longer using MagicalRecord

## State of Magical Record

[Magical Record](https://github.com/magicalpanda/MagicalRecord/) is an awesome library that makes life much easier when you use Core Data.

However, the project is pretty stagnant. 

[One year ago](https://github.com/magicalpanda/MagicalRecord/wiki/Upgrading-to-MagicalRecord-3.0), they started work on version 3, yet it is still not finished. And it will continue to be in Objective-C.

As such, I have _deprecated_ Magical Record for my new projects. Instead, I use Core Data stack directly, which actually isn't too bad after having experience studying how Magical Record (and others) work.


## 1. Setup NSFetchedResultsController

We use the example of a `Food` model, which has the attributes `type` and `createdAt`.

```swift
lazy var fetchedResultsController: NSFetchedResultsController = {
    let fetchRequest = NSFetchRequest(entityName: "Food")
    fetchRequest.fetchLimit = 100
    fetchRequest.fetchBatchSize = 20

    // Filter Food where type is breastmilk
    var predicate = NSPredicate(format: "%K == %@", "type", "breastmilk")
    fetchRequest.predicate = predicate

    // Sort by createdAt
    fetchRequest.sortDescriptors = [NSSortDescriptor(key: "createdAt", ascending: false)]

    let frc = NSFetchedResultsController(fetchRequest: fetchRequest, managedObjectContext: DaRecord.mainContext, sectionNameKeyPath: nil, cacheName: nil)
    frc.delegate = self
    return frc
}()
```

Thanks to Swift, we now can simply declare `fetchedResultsController` property as `lazy`.


## 2. Setup Your View Controller viewDidLoad

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    
    do {
        try fetchedResultsController.performFetch()
    } catch {
        print("Error")
    }
}
```

When the view is loaded, we perform the fetch once.


## 3. The Delegates

There will be a 3 delegates needed (for `NSFetchedResultsController` and `UITableView`):

```swift
class YourViewController: UIViewController, NSFetchedResultsControllerDelegate, UITableViewDataSource, UITableViewDelegate 
```

We will see how to implement them in the next 3 sections.


### 3a. UITableViewDataSource

`UITableViewDataSource` will ask for the data. 

Not surprisingly, in all of the methods to implement, we will ask `fetchedResultsController`, which holds the fetched data.

```swift    
func numberOfSectionsInTableView(tableView: UITableView) -> Int {
    if let sections = fetchedResultsController.sections {
        return sections.count
    }
    return 0
}

func tableView(tableView: UITableView, titleForHeaderInSection section: Int) -> String? {
    if let currSection = fetchedResultsController.sections?[section] {
        return currSection.name
    }
    return nil
}

func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    if let currSection = fetchedResultsController.sections?[section] {
        return currSection.numberOfObjects
    }
    return 0
}

func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
    let cell = tableView.dequeueReusableCellWithIdentifier("food-cell", forIndexPath: indexPath)
    configureCell(cell, atIndexPath: indexPath)
    return cell
}

// A private method to configure cell at indexPath
func configureCell(cell: UITableViewCell, atIndexPath indexPath: NSIndexPath) {
    // Configure cell with the food model
    let food = fetchedResultsController.objectAtIndexPath(indexPath) as! Food
    cell.textLabel?.text = food.type
    cell.detailTextLabel?.text = String(format: "%@", food.createdAt!)
}
```


## 3b. NSFetchedResultsControllerDelegate

Using `NSFetchedResultsControllerDelegate`, you can know if a model is inserted/deleted/updated/moved, then update the table view. 

Quite a chunk of code that you can simply copy and paste, without any modification:

```swift
func controller(controller: NSFetchedResultsController, didChangeSection sectionInfo: NSFetchedResultsSectionInfo, atIndex sectionIndex: Int, forChangeType type: NSFetchedResultsChangeType) {
    switch(type) {
    case .Insert:
        tableView.insertSections(NSIndexSet(index: sectionIndex), withRowAnimation: UITableViewRowAnimation.Fade)
    case .Delete:
        tableView.deleteSections(NSIndexSet(index: sectionIndex), withRowAnimation: UITableViewRowAnimation.Fade)
    default:
        break
    }
}

func controller(controller: NSFetchedResultsController, didChangeObject anObject: AnyObject, atIndexPath indexPath: NSIndexPath?, forChangeType type: NSFetchedResultsChangeType, newIndexPath: NSIndexPath?) {
    switch(type) {
    case .Insert:
        if let newIndexPath = newIndexPath {
            tableView.insertRowsAtIndexPaths([newIndexPath], withRowAnimation:UITableViewRowAnimation.Fade)
        }
    case .Delete:
        if let indexPath = indexPath {
            tableView.deleteRowsAtIndexPaths([indexPath], withRowAnimation: UITableViewRowAnimation.Fade)
        }
    case .Update:
        if let indexPath = indexPath {
            if let cell = tableView.cellForRowAtIndexPath(indexPath) {
                configureCell(cell, atIndexPath: indexPath)
            }
        }
    case .Move:
        if let indexPath = indexPath {
            if let newIndexPath = newIndexPath {
                tableView.deleteRowsAtIndexPaths([indexPath], withRowAnimation: UITableViewRowAnimation.Fade)
                tableView.insertRowsAtIndexPaths([newIndexPath], withRowAnimation: UITableViewRowAnimation.Fade)
            }
        }
    }
}

func controllerWillChangeContent(controller: NSFetchedResultsController) {
    tableView.beginUpdates()
}

func controllerDidChangeContent(controller: NSFetchedResultsController) {
    tableView.endUpdates()
}
```

## 3c. UITableViewDelegate

If you require deleting, add this:

```swift
func tableView(tableView: UITableView, commitEditingStyle editingStyle: UITableViewCellEditingStyle, forRowAtIndexPath indexPath: NSIndexPath) {
    switch editingStyle {
    case .Delete:
        print("TODO: Delete model")
    case .Insert:
        print("TODO: Insert model")
    default: break
    }
}
```



