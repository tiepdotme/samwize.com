---
layout: post
title: "UndoManager"
date: 2019-02-16T10:49:00+08:00
categories: [Foundation]
---

[`UndoManager`](https://developer.apple.com/documentation/foundation/undomanager) in part of Foundation, thus available for both iOS, macOS, etc.

## Registering an undo action

Whenever you have an action that you want to undo, you will register a closure with your undo implementation, via an instance of `undoManager` (which UI/NSResponder holds one).

Let's use an example whereby a user "add" an item, where the undo is to "remove".

```swift
@IBAction func add(_ sender: Any) {
    let newModel = ...

    undoManager?.registerUndo(withTarget: self) { target in
        target.removeObject(newModel)
    }
    undoManager?.setActionName("Add")

    addObject(newModel)
}
```

`addObject` and `removeObject` are your custom functions to add/remove the model object.

[`registerUndo`](https://developer.apple.com/documentation/foundation/undomanager/2427208-registerundo) will tell `undoManager` how to perform the undo, by running the closure. Typically you can use the view controller, or any controller, as the target.

Note that `undoManager` does not hold a strong reference to target, so [feel safe to use without specifying _weak self_](/2016/08/05/reference-cycle-for-closures/).

You have to set the action name (a localized `String`), as it will be used as a [display text](https://developer.apple.com/design/human-interface-guidelines/ios/user-interaction/undo-and-redo/) to user, with the prefix "Undo", eg "Undo Add".

When user press _Cmd+Z_, or shake the iPhone, the undo operation will be performed.

## Registering a redo action

> Redo = undo an undo

To have a redo, you have to register an undo in the undo closure.

We can improve the code, and abstract with a new func `addObjectWithUndo`.

```swift
@IBAction func add(_ sender: Any) {
    let newModel = ...
    addObjectWithUndo(newModel)
}

// Register the undo operation and add the object model
private func addObjectWithUndo(_ object: Any) {
    undoManager?.registerUndo(withTarget: self) { target in
        // Call the corresponding `removeObjectWithUndo`, which also register another undo.
        // Remember: redo = undo an undo
        target.removeObjectWithUndo(object)
    }
    undoManager?.setActionName("Add")

    addObject(object)
}

private func removeObjectWithUndo(_ object: Any) {
    undoManager?.registerUndo(withTarget: self) { target in
        target.addObjectWithUndo(object)
    }
    undoManager?.setActionName("Remove")

    removeObject(newModel)
}
```

The pair of add and remove func will provide the redo capability.

## Pitfall: Run loop undo all instead of popping one

In our example, there is a critical pitfall. If you were to add multiple times, then undo, you expect to pop the last undo operation off the stack.

However, the example is buggy (on purpose), and ALL will be undo because:

> NSUndoManager normally creates undo groups automatically during a cycle of the run loop. The first time it is asked to record an undo operation in the cycle, it creates a new group. Then, at the end of the cycle, it closes the group. You can create additional, nested undo groups.

By default, [`groupsByEvent`](https://developer.apple.com/documentation/foundation/undomanager/1417407-groupsbyevent) is `true`. This creates the group automatically in a [run loop](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/RunLoopManagement/RunLoopManagement.html).

To "fix", you should set `groupsByEvent` to `false.`, and specify when you begin/end the groping manually.

```swift
private func addObjectWithUndo(_ object: Any) {
    undoManager?.groupsByEvent = false
    undoManager?.beginUndoGrouping()
    undoManager?.registerUndo(...)
    undoManager?.setActionName("Add")
    undoManager?.endUndoGrouping()
    ...
}
```

That 5 lines of code can be abstracted for every time you want to register an undo (:

## Design Patterns

[iOS has design patterns](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/UndoArchitecture/Articles/iPhoneUndo.html) in regards to undo/redo. Usually, undo is unnecessary as edits are committed immediately.
