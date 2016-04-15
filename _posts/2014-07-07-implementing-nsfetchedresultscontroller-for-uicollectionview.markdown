---
layout: post
title: "Implementing NSFetchedResultsController for UICollectionView"
date: 2014-07-07 16:43:56 +0800
comments: true
categories: iOS
---

I previously wrote about [how to implement NSFetchedResultsController with Magical Record (UITableView version)](http://samwize.com/2014/03/29/implementing-nsfetchedresultscontroller-with-magicalrecord/).

For `UICollectionView`, it is largely the same.

Except for the change that Apple did - there is NO `[collectionView beginUpdates/endUpdates]` methods.

<!-- more -->

Therefore, a working implementation for the 4 Crazy NSFetchedResultsControllerDelegate methods is needed to the [earlier post](http://samwize.com/2014/03/29/implementing-nsfetchedresultscontroller-with-magicalrecord/).

The code below is exactly from [Jose](http://jose-ibanez.tumblr.com/post/38494557094/uicollectionviews-and-nsfetchedresultscontrollers), who adapted the code from [AshFurrow](https://github.com/AshFurrow/UICollectionView-NSFetchedResultsController/blob/master/AFMasterViewController.m). Credits to them.

Firstly, you have to add 2 `NSMutableArray` to your view controller to store the changes.

```objc
@interface MyViewController ()
@property NSMutableArray *sectionChanges;
@property NSMutableArray *itemChanges;
@end
```

In the 4 Crazy NSFetchedResultsControllerDelegate methods, you bascially add to the arrays, and finally update them in batches in `controllerDidChangeContent`.

```objc
- (void)controllerWillChangeContent:(NSFetchedResultsController *)controller {
    _sectionChanges = [[NSMutableArray alloc] init];
    _itemChanges = [[NSMutableArray alloc] init];
}

- (void)controller:(NSFetchedResultsController *)controller
  didChangeSection:(id <NSFetchedResultsSectionInfo>)sectionInfo
           atIndex:(NSUInteger)sectionIndex
     forChangeType:(NSFetchedResultsChangeType)type {
    NSMutableDictionary *change = [[NSMutableDictionary alloc] init];
    change[@(type)] = @(sectionIndex);
    [_sectionChanges addObject:change];
}

- (void)controller:(NSFetchedResultsController *)controller
   didChangeObject:(id)anObject
       atIndexPath:(NSIndexPath *)indexPath
     forChangeType:(NSFetchedResultsChangeType)type
      newIndexPath:(NSIndexPath *)newIndexPath {
    NSMutableDictionary *change = [[NSMutableDictionary alloc] init];
    switch(type) {
        case NSFetchedResultsChangeInsert:
            change[@(type)] = newIndexPath;
            break;
        case NSFetchedResultsChangeDelete:
            change[@(type)] = indexPath;
            break;
        case NSFetchedResultsChangeUpdate:
            change[@(type)] = indexPath;
            break;
        case NSFetchedResultsChangeMove:
            change[@(type)] = @[indexPath, newIndexPath];
            break;
    }
    [_itemChanges addObject:change];
}

- (void)controllerDidChangeContent:(NSFetchedResultsController *)controller {
    [self.collectionView performBatchUpdates:^{
        for (NSDictionary *change in _sectionChanges) {
            [change enumerateKeysAndObjectsUsingBlock:^(id key, id obj, BOOL *stop) {
                NSFetchedResultsChangeType type = [key unsignedIntegerValue];
                switch(type) {
                    case NSFetchedResultsChangeInsert:
                        [self.collectionView insertSections:[NSIndexSet indexSetWithIndex:[obj unsignedIntegerValue]]];
                        break;
                    case NSFetchedResultsChangeDelete:
                        [self.collectionView deleteSections:[NSIndexSet indexSetWithIndex:[obj unsignedIntegerValue]]];
                        break;
                }
            }];
        }
        for (NSDictionary *change in _itemChanges) {
            [change enumerateKeysAndObjectsUsingBlock:^(id key, id obj, BOOL *stop) {
                NSFetchedResultsChangeType type = [key unsignedIntegerValue];
                switch(type) {
                    case NSFetchedResultsChangeInsert:
                        [self.collectionView insertItemsAtIndexPaths:@[obj]];
                        break;
                    case NSFetchedResultsChangeDelete:
                        [self.collectionView deleteItemsAtIndexPaths:@[obj]];
                        break;
                    case NSFetchedResultsChangeUpdate:
                        [self.collectionView reloadItemsAtIndexPaths:@[obj]];
                        break;
                    case NSFetchedResultsChangeMove:
                        [self.collectionView moveItemAtIndexPath:obj[0] toIndexPath:obj[1]];
                        break;
                }
            }];
        }
    } completion:^(BOOL finished) {
        _sectionChanges = nil;
        _itemChanges = nil;
    }];
}
```

## Pitfall: fetchLimit

If you use `fetchLimit` when you `performFetch`, you might be thinking it will be "honoured" throughout the `NSFetchedResultsController` eg. if they is change in the data, it will still be limited

Unfortunately, it [does not](http://stackoverflow.com/q/9022580/242682) limit the number of records.

A [quick solution](http://stackoverflow.com/a/9025638/242682) is to `performFetch` again, whenever there is a change in content:

```swift
override func controllerDidChangeContent(controller: NSFetchedResultsController) {
    _ = try? fetchedResultsController.performFetch()
}
```

Another [solution](http://stackoverflow.com/a/10837871/242682) is to limit the number of cells to display in the `UICollectionViewDataSource` methods. (Disclaimer: I didn't have success with that, as the code produces "CoreData: error: Serious application error... controllerDidChangeContent...", but the concept is correct)
