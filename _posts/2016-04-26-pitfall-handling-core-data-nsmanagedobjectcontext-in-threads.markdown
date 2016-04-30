---
layout: post
title: "Pitfall: Handling Core Data NSManagedObjectContext in Threads"
date: 2016-04-26T10:42:28+08:00
categories: [iOS]
---

It is very common to have Core Data crashing, especially if you are using multiple contexts in multi-threaded environment.

If you have a crash in a background thread (eg `com.apple.root.background-qos`) and the crash is from Core Data framework, it is likely you are using a `NSManagedObjectContext` wrongly.


## Wrap in `performBlockAndWait`

When you create a private context, eg:

```swift
let context = NSManagedObjectContext(concurrencyType: .PrivateQueueConcurrencyType)
```

You should always wrap any stuff you do with the context in a `performBlockAndWait`, or `performBlock`.

This is **crucial**, and most often omitted.

```swift
context.performBlockAndWait({ 
    // Do something with the models in context
    ...
    
    // Finally, save it, if needed
    do {
      try context.save()
    } catch {}
})
```

By wrapping in `performBlockAndWait`, it will make sure any code in the block is executed in the right thread. 


## Debugging

iOS/OSX used to throw an exception for every threading violation. But Apple has to turned off the feature because that will crash too many apps that were in production.

We can [turn on](http://oleb.net/blog/2014/06/core-data-concurrency-debugging/) the feature.

Go to Edit Scheme > Run > Arguments > Add `-com.apple.CoreData.ConcurrencyDebug 1` to Arguments Passed on Launch.

Run your app, and if you violated the threading policy, the app will crash!

`__Multithreading_Violation_AllThatIsLeftToUsIsHonor__`
