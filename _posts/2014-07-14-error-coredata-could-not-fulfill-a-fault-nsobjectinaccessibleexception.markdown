---
layout: post
title: "Error: CoreData could not fulfill a fault. NSObjectInaccessibleException"
date: 2014-07-14 14:06:35 +0800
comments: true
categories: [iOS, CoreData]
---

One of the common errors when using CoreData is 

```
*** Terminating app due to uncaught exception 'NSObjectInaccessibleException', reason: 'CoreData could not fulfill a fault for '0x10123456 <x-coredata://3E1F9B1C-46BE-49F8-A3ED-1234567BF0/Event/p42>```

This is what happened:

<!-- more -->

CoreData tried to access the entity, but it could not even fulfill a fault. It is most likely because the entity is no longer around, aka deleted.

Read [Apple CoreData troubleshooting guide](https://developer.apple.com/library/mac/documentation/cocoa/conceptual/coredata/articles/cdTroubleshooting.html) on such object life-cycle problem.

I have encountered this more than once when using `NSFetchedResultsController` and with `Magical Record`.

- `NSFetchedResultsController` is using it's a context for fetching the entities

- `Magical Record` is using another context to update/delete some entities

- When `Magical Record` deletes an entity, `NSFetchedResultsController` has the (crashing) error

The solution is to be careful with using multiple contexts and avoid the scenario. 

Duh.

Okay, let me give you a more meaningful workaround: If you are deleting, you might want to "mark for delete" instead. Then let `NSFetchedResultsController` to run at some interval to remove the marked entities.