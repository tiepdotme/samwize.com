---
layout: post
title: "Guide to Binding NSTableView to Core Data"
date: 2018-12-06T15:06:30+08:00
categories: [macOS]
---

You can populate a table view with full CRUD without writing any code.

Ok, no/not much code, but you need to set up quite a few things with Xcode interface builder. It ain't easy to navigate in Xcode, so here is a step by step guide.

## Array Controller

1. Add an **Array Controller** to the storyboard scene
2. In Attributes inspector, set **Mode** to Entity Name and enter your entity model name
3. Enable **Prepares Content** so that that the table will load content automatically
4. In Bindings inspector, enable bind to for **Managed Object Context** and select your scene
5. Set **Model Key Path** to `self.context`

In your view controller, you will need to init the context property. You can read this post on [my modern Core Data stack](/2018/09/01/modern-guide-to-core-data-2018/).

```swift
class ExchangeRateViewController: NSViewController {

    @objc dynamic var context: NSManagedObjectContext!

    override func viewDidLoad() {
        super.viewDidLoad()
        self.context = ... // Your Core Data stack viewContext
    }

}
```

## Table View (Cell Based)

Let's configure for a simple case using Cell Based table view. We cover View Based table next, which is the modern way and allow you to use any kind of view in each cell. The Cell Based way is only good for simple text fields.

1. In Attributes inspector, set **Content Mode** to **Cell Based**.
2. Select a column to bind
3. Under Bindings inspector, enable for **Value** and bind to **Array Controller**
4. The Controller Key is `arrangedObjects`
5. The Model Key Path is your entity model attribute name
6. Repeat for all columns

Run, and you will be able to edit the fields!

Ok, here's the caveat: if your attribute is a String type, it works perfectly. For other types, you need a value transformer. Read it in next section.

But before we go on to value transformer, let's know how to configure for View Based table view.

## Table View (View Based)

1. In Attributes inspector, set **Content Mode** to **View Based**.
2. In Bindings inspector, bind **Content** to Array Controller. The Controller Key should already be `arrangedObjects`.
3. Select the actual view to bind eg. `NSTextField`, `NSDatePicker` etc
4. In Bindings inspector, bind **Value** to **Table Cell View**
5. Set the **Model Key Path** to `objectValue.attributeName` (your entity attribute name!)

The most tricky part is in step (3) -- selecting the correct view.

In a View Based table, you have to bind the actual view. You must select the correct view, which is in this complex hierarchy `NSTableColumn` > `NSTableCellView` > The actual view eg. `NSTextField` > `NSTextFieldCell`.

## Value Transformer

When using Cocoa Binding, the data type might need some massaging. Value Transformer is 1 one of the concept that transforming the type. There is also formatters, which is more for formatting as String for display.

The [full binding flow](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CocoaBindings/Concepts/MessageFlow.html) is daunting.

Creating a value transformer includes subclassing `ValueTransformer` and overriding 3 methods, and then register it with a name (String). I will not go into [the details](https://nshipster.com/valuetransformer/).

## Add & Remove

It is common to have gradient buttons (the + and - square buttons) below the table view to easily add row or delete selected rows.

1. Add a Gradient Button
2. Under Connections inspector, drag the **action** to Array Controller, and select `add:`
3. Repeat the same for the delete button but select `remove:`

But if you want more controls over adding, you might want to write your own methods. In the example below, I add a new model with a default date to now.

```swift
@IBAction func add(_ sender: Any) {
    let newModel = arrayController.newObject() as AnyObject
    newModel.setValue(Date(), forKey: "date")
    arrayController.addObject(newModel as Any)
}

@IBAction func remove(_ sender: Any) {
    // Do certain stuff before removing all selected rows
    arrayController.remove(atArrangedObjectIndexes: arrayController.selectionIndexes)
}
```

For this to work, you also need to set up binding to the table view's selections:

1. Select the table view > Binding inspector > Selection Indexes > Bind to the Array Controller
2. Set Controller Key to `selectionIndexes`

## Sort & Filter

This binds Array Controller to your `NSSortDescriptor`:

1. Select Array Controller > Bindings inspector > Sort Descriptors > Bind to the view controller
2. Set Model Key Path to `self.sorts`
3. In your view controller, add the instance var `@objc dynamic var sorts: [NSSortDescriptor]!`
4. Init `sorts` with an array `NSSortDescriptor` in `viewDidLoad`

Filter is similar, binding to the Filter Predicate.

With that, the table view content will sort and filter accordingly.

To sort by the table column, you need to these additionally:

1. Select Table View > Bindings inspector > Sort Descriptors > Bind to the view controller
2. Set Model Key Path to `self.sorts`
3. Select a table column > Bindings inspector > Value > Bind to Array Controller
4. Set the model key path eg. "date"
5. Make sure Creates Sort Descriptor is enabled

With that, you can now click on a header and sort it ascending/descending.

## Conclusion

If you have followed the steps so far, you should find using Xcode interface builder to set up binding is a pain.. I would rather code than set up so many fields. In time to come, it is easy to forget how certain things are wired up.

As such, the alternative is to take away binding and array controller. Instead, simply use Core Data's `NSFetchedResultsController` together with the `NSTableView` via the [regular delegate and datasource patterns](/2018/11/16/guide-to-nsfetchedresultscontroller-with-nstableview-macos/).
