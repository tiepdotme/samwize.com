---
layout: post
title: "Binding With Array Controller"
date: 2019-04-05T11:43:10+08:00
categories: [macOS]
---

In my [first guide to binding](/2018/12/06/guide-to-binding-nstableview-to-nsfetchedresultscontroller/), it uses `NSFetchedResultsController`, which is tightly coupled to the Core Data framework.

It is not necessary (of course) to bind to Core Data models.

This guide will be more generic, providing a guide to binding to a plain old object model.

## The Model

Let's use a simple model `MyData`.

```swift
class MyData: NSObject {
    @objc var name: String
}
```

Any member variable such as `name` has to be annotated with `@objc`, because that's how the old world works and provides.

## Array Controller

`NSArrayController` manages the models.

It does NOT hold the models, but simply manages it in terms of sorting, filtering, and CRUD operations.

It is common to add the `NSArrayController` object in a storyboard. Then set up as such:

- In Attributes Inspector:
  - Mode is class
  - Class name is `MyApp.MyData`
  - The prefix `MyApp` is necessary, and it is your target's **Product Module Name**
- In Binding Inspector > Controller Content:
  - Bind to your view controller (usually) eg. `MyListViewController`
  - Model Key Path is `self.myDataList`

## View Controller

It is a good time now to show you what your view controller looks like.

```self
class MyListViewController: NSViewController {
    @objc dynamic var myDataList = [MyData]()
    @objc dynamic var sorts = [NSSortDescriptor]()
    @objc dynamic var filter: NSPredicate?
}
```

`myDataList` is the actual models that the view controller holds. It must be prefix with `@objc` once again.

Remember: The `NSArrayController` is managing these models. You don't mutate the array controller directly.

Whenever you want to update your model, mutate `myDataList`.

## Table View

Next, we bind the array controller to the table view.

1. In Attributes inspector, set **Content Mode** to **View Based**.
2. In Bindings inspector, bind **Content** to Array Controller. The Controller Key should already be `arrangedObjects`.
3. Select the actual view to bind eg. `NSTextField`, `NSDatePicker` etc
4. In Bindings inspector, bind **Value** to **Table Cell View**
5. Set the **Model Key Path** to `objectValue.name` (your model's attribute!)

## Sort & Filter

1. **Select Array Controller** > Bindings inspector > Sort Descriptors > Bind to the view controller
2. Set Model Key Path to `self.sorts`
3. Similarly, for filter bind to `self.filter`
4. Init `sorts` and `filter` in `viewDidLoad`
5. Mutate them, and the views will be updated immediately

### Sort when clicking on the table column header

1. **Select Table View** > Bindings inspector > Sort Descriptors > Bind to the view controller
2. Set Model Key Path to `self.sorts`
3. Select a table column > Bindings inspector > Value > Bind to Array Controller
4. Set the model key path eg. "name"
5. Make sure Creates Sort Descriptor is enabled

## Selection

When you select rows in the table view, you should bind it to the array controller too.

1. Select Table View > Bindings inspector > Selection Indexes > Bind to the Array Controller
2. Set Controller Key to `selectionIndexes`

## Delete Selected Rows

```swift
@IBAction func deleteSelectedRows(_ sender: Any) {
    arrayController.remove(atArrangedObjectIndexes: arrayController.selectionIndexes)
}
```

I have omitted the obvious: Add a button with the action to the above function, adding the array controller outlet to the view controller.

## Add New Row

Similarly, you can have a button to add new model(s).

```swift
@IBAction func add(_ sender: Any) {
    let newModel = MyData()
    arrayController.addObject(newModel)
}
```
