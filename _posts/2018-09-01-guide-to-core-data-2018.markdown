---
layout: post
title: "Guide to Core Data 2018"
date: 2018-09-01T13:25:29+08:00
categories: [Database]
---

Core Data has been around for 10 years, with many legacy concepts and APIs. This guide is the modern way to use Core Data, until further WWDCs :)

If you are interested in the history of how we got here, the last section has the long history, describing the stack and libraries to use, and the issues.

The technology has since improved much. In this guide, I will use only what a modern developer should use.

## Create the database

Use [Xcode](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreData/KeyConcepts.html) to create the model schema. In this guide, the data model name (the .xcdatamodeld file) is "MyDataModel".

When editing an entity, don't do extra, and leave the entity name and class name the **same**.

Set **Codegen** to **Class Definition**.

Throughout this guide, we will use the following Note model as example:

```swift
@objc(Note)
public class Note: NSManagedObject { }
extension Note {
    @NSManaged public var content: String?
    @NSManaged public var priority: NSNumber?
}
```

There is also a generated `fetchRequest` with the Note type, which I have omitted above, but you should not remove in your code.

## Setup the stack

```swift
public class DB {

    public static let `default` = DB()

    public var container: NSPersistentContainer!

    /// Call this once in `applicationDidFinishLaunching`
    public func setup(dataModelName: String) {
        container = NSPersistentContainer(name: dataModelName)
        container.loadPersistentStores { _, error in
            if let error = error {
                fatalError("Unable to load persistent stores: \(error)")
            }
        }
    }

}
```

We create a singleton class `DB.default` which we will use throughout the app. The `setup` must be run when the application is launched, with "MyDataModel" name.

It will crash if there is error -- and yes, you want it to crash because most apps cannot continue without a working database.

If you have an advanced/unconventional use case, [`NSPersistentContainer`](https://developer.apple.com/documentation/coredata/nspersistentcontainer) has other inits that will allow you to customize the data model to load, and configure the persistent store.

## CRUD

### Create (aka save)

The steps in essence:

1. Perform everything in a block with background context
2. Create a new Note (`NSManagedObject` object) in the context
3. Save the context

```swift
DB.default.container.performBackgroundTask { context in
    let note = Note(context: context)
    note.content = "Hello World"
    note.priority = 99
    try! context.save()
}
```

This is the modern way, which is much shorter than the past. If you don't believe, take a look at [Ray Wenderlich's](https://www.raywenderlich.com/353-getting-started-with-core-data-tutorial) "updated guide" in 2017, which still uses the tedious way involving `NSEntityDescription.entity` and `NSManagedObject(entity:insertInto:)`.

What happens when a context is saved? It will [commit "one store up"](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext), to either a parent context or the actual persistent store.

### Read (aka fetch)

The steps in essence:

1. Construct a fetch request (made up of predicates and sort descriptors)
2. Use either `viewContext` (main thread) or `newBackgroundContext()`
3. Call `fetch` with the fetch request

```swift
let fetchRequest: NSFetchRequest<Note> = Note.fetchRequest()
fetchRequest.predicate = ...
fetchRequest.sortDescriptors = ...
let notes = try! DB.default.container.viewContext.fetch(fetchRequest)
notes.forEach {
    print($0.content)
}
```

If you didn't set any predicate, the fetch request will be to fetch all notes. Learning predicate will be another topic for another day. If you want to learn, you may refer to [the](https://developer.apple.com/documentation/foundation/nspredicate) [documentation](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Predicates/Articles/pSyntax.html), [guide](https://nshipster.com/nspredicate/) and [cheatsheet](https://academy.realm.io/posts/nspredicate-cheatsheet/).

You could also call [`fetchRequest.execute()`](https://developer.apple.com/documentation/coredata/nsfetchrequest/1640594-execute), which will automatically use the context associated on the current thread. Not recommended unless you are sure of the threads.

### Update

Updating is simply mutating the models, then saving the context.

```swift
DB.default.container.performBackgroundTask { context in
    let fetchRequest: NSFetchRequest<Note> = Note.fetchRequest()
    fetchRequest.predicate = ...
    let notes = try! context.fetch(fetchRequest)
    let note = notes.first
    note.content = "This is the first note"
    try! context.save()
}
```

The example above fetch notes in the background contexts, then mutate the first note and save.

### Delete

Once again, use context to `delete` then `save`.

```swift
DB.default.container.performBackgroundTask { context in
    ...
    context.delete(note)
    try! context.save()
}
```

Note that if you need to delete all notes, you need to fetch all of them and calling delete for each. This is inefficient since you would need to load all the models, which is unnecessary in a delete. You could set `includesPropertyValues` to false in the fetch request. Another way is to use [batch processing](https://developer.apple.com/documentation/coredata/batch_processing).

## Dealing with concurrency

The introduction of container simplified the framework, by making developer choose between these two kind of contexts:

1. [`viewContext`](https://developer.apple.com/documentation/coredata/nspersistentcontainer/1640622-viewcontext) is in main thread, and is READ only; you cannot call `save`.
2. `newBackgroundContext()` or `performBackgroundTask` is in background thread

![Container and contexts](/images/core-data-container.png)

The parent of both `viewContext` and `newBackgroundContext()` is the persistent store. As said before, when you save a context, it will commit to the parent.

When you save a background context, it will save to the persistent store, but it will NOT merge to the main context.

Often you would want your main context to reflect changes. To do [that](https://stackoverflow.com/q/39348729/242682), you have to configure `viewContext` when setting up your database:

```swift
container.viewContext.automaticallyMergesChangesFromParent = true
```

If you perform save concurrently in multiple contexts, you could have merge conflicts. One way is to have an [operation queue](https://stackoverflow.com/a/42745378/242682).

What happens to existing fetched objects when a merge happens? They are not affected. You need to refresh the changes by executing the fetch again.

How to know a context has changes? Observe posted notifications such as [`NSManagedObjectContextDidSave`](https://developer.apple.com/documentation/foundation/nsnotification/name/1506380-nsmanagedobjectcontextdidsave) and deal with the inserted, updated and deleted objects.

## Pitfall: Faults

When you fetch models, sometimes there will be faults.

Faults are "unrealized objects", designed to make Core Data efficient by needless fetching, until needed.

Faults are automatically resolved (fetched) when you access the property.

But if the "unrealized object" is somehow deleted? Crash could occur. A simple solution below to make those faults nil instead.

```swift
context.shouldDeleteInaccessibleFaults = true
```

## Query Generation

It's a good time to know this new feature in iOS 10. It prevents faults and crashes. Read this [guide](https://cocoacasts.com/what-are-core-data-query-generations/) and watch [WWDC 2016](https://developer.apple.com/videos/play/wwdc2016/242/).

In essence, each context is pinned to a snapshot of the database.

By default, context are unpinned. You can start pinning with:

```swift
let token = context.queryGenerationToken
context.setQueryGenerationFrom(token)
```

At some point in time, you could move to the latest snapshot with `NSQueryGenerationToken.current`.

## Other topics

[`NSFetchedResultsController`](https://developer.apple.com/documentation/coredata/nsfetchedresultscontroller) manage the results of a fetch request, including changes to the objects in the context! A big topic so I leave in to another day. For now, you can read my [2015 guide](/2015/10/27/implementing-nsfetchedresultscontroller-in-swift/).

[Migration](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreDataVersioning/Articles/Introduction.html) is unavoidable in app upgrade.

## Long bit of history

I use Core Data way back in 2009 when iOS was first launch.

Why use Core Data at all? Because with Core Data API you can **avoid writing SQL statements**. That is the biggest benefit. A modern way to write database code.

But there are still lots of pain with this piece of Apple technology.

Over 10 years, it did improve, though some updates are long overdue considering it is a vital piece in the iOS/macOS stack.

- 2009: Core Data in iOS 3
- 2010: [MagicalRecord](https://github.com/magicalpanda/MagicalRecord) is THE wrapper, was in Objective-C, but now dormant.
- 2010: [mogenerator](https://github.com/rentzsch/mogenerator) is the third party model generator
- 2015: [CoreStore](https://github.com/JohnEstropia/CoreStore) is in Swift and still updated, but might not be using the latest concepts
- 2016: New stuff announced in WWDC

Back in the days.. my stack is to use [MagicalRecord + mogenerator](https://samwize.com/2014/03/27/step-by-step-guide-to-using-magicalrecord-and-mogenerator/). There [are](/2013/09/03/where-to-store-coredata-sqlite-file-and-avoid-apple-app-review-rejection/) [many](/2015/06/02/pitfall-creating-parent-slash-abstract-entitiy-in-core-data/) [pitfalls](/2014/07/04/the-rules-for-using-external-storage-in-core-data/) eg. [concurrency in managed object context](/2016/04/26/pitfall-handling-core-data-nsmanagedobjectcontext-in-threads/), [faults](/2014/07/14/error-coredata-could-not-fulfill-a-fault-nsobjectinaccessibleexception/).

But Apple has fixed some quirks, at last.

In [WDDC 2016](https://developer.apple.com/videos/play/wwdc2016/242/), Apple has a pivotal release with the concept of `NSPersistContainer`, wrapping the creation of a database stack, and using that same container to access either a main context or a background context.

The managed object class generation is built in, with those sensible methods such as `entity()`. The use of Swift generic make type casting unnecessary.

Suddenly, Core Data seems much nicer to play with.

Yet there are more that can be improved.
