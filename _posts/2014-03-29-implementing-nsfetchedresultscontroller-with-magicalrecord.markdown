---
layout: post
title: "Implementing NSFetchedResultsController with MagicalRecord"
date: 2014-03-29 18:52:52 +0800
comments: true
categories: [iOS]
---

This is an adaptation of a [cheat sheet on iosdevblog.com](http://iosdevblog.com/tag/nsfetchedresultscontroller/).

The original post provides the code to implement `NSFetchedResultsController`, but without [MagicalRecord](https://github.com/magicalpanda/MagicalRecord).

This post will provide the code with MagicalRecord.

<!-- more -->

## Declare a NSFetchedResultsController and delegate

```objc
@interface PoosViewController : UIViewController <UITableViewDataSource, UITableViewDelegate, NSFetchedResultsControllerDelegate>
@property (strong, nonatomic) NSFetchedResultsController *fetchedResultsController;
@property (weak, nonatomic) IBOutlet UITableView *tableView;
@end
```

In this post, I am using an example of a `UIViewController` call `PoosViewController`, which display some `Poo` model in a `UITableView`.


## Setup a simple NSFetchedResultsController

```objc
- (NSFetchedResultsController *)fetchedResultsController {
    if (_fetchedResultsController != nil) {
        return _fetchedResultsController;
    }

    _fetchedResultsController = [Poo fetchAllSortedBy:@"pooedOn" ascending:NO withPredicate:nil groupBy:nil delegate:self];
    return _fetchedResultsController;
}
```


## Setup a complex NSFetchedResultsController

However, if you need more complex fetching eg. limit fetch results, then you need to use `NSFetchRequest` directly.

```objc
- (NSFetchedResultsController *)fetchedResultsController {
    if (_fetchedResultsController != nil) {
        return _fetchedResultsController;
    }

    NSFetchRequest *fetchRequest = [Poo requestAllSortedBy:@"pooedOn" ascending:NO];
    [fetchRequest setFetchLimit:100];         // Let's say limit fetch to 100
    [fetchRequest setFetchBatchSize:20];      // After 20 are faulted

    NSFetchedResultsController *theFetchedResultsController = [[NSFetchedResultsController alloc] initWithFetchRequest:fetchRequest managedObjectContext:[NSManagedObjectContext defaultContext] sectionNameKeyPath:nil cacheName:@"PooCache"];
    self.fetchedResultsController = theFetchedResultsController;
    self.fetchedResultsController.delegate = self;
    return _fetchedResultsController;
}
```


## Perform a fetch in viewDidLoad

```objc
- (void)viewDidLoad {
    [super viewDidLoad];

    // Delete cache first, if a cache is used
    [NSFetchedResultsController deleteCacheWithName:@"PooCache"];
    NSError *error;
    if (![[self fetchedResultsController] performFetch:&error]) {
        // Update to handle the error appropriately.
        NSLog(@"Unresolved error %@, %@", error, [error userInfo]);
    }
}
```


## Table view data source

```objc
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return [[_fetchedResultsController sections] count];
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {
    id sectionInfo = [[_fetchedResultsController sections] objectAtIndex:section];
    return [sectionInfo numberOfObjects];
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
    static NSString *CellIdentifier = @"PooCell";
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:CellIdentifier forIndexPath:indexPath];

    // Configure the cell
    [self configureCell:cell atIndexPath:indexPath];

    return cell;
}
```


## Helper method to configure cell

```objc
- (void)configureCell:(UITableViewCell*)cell atIndexPath:(NSIndexPath*)indexPath {
  Poo *poo = [self.fetchedResultsController objectAtIndexPath:indexPath];
  // Update cell with poo details
  cell.textLabel.text = poo.title;
}
```


## Deleting

```objc
- (void)tableView:(UITableView *)tableView commitEditingStyle:(UITableViewCellEditingStyle)editingStyle forRowAtIndexPath:(NSIndexPath *)indexPath {
    if (editingStyle == UITableViewCellEditingStyleDelete) {
        // The correct way to save (http://samwize.com/2014/03/29/how-to-save-using-magicalrecord/)
        Poo *poo = [self.fetchedResultsController objectAtIndexPath:indexPath];
        [MagicalRecord saveWithBlock:^(NSManagedObjectContext *localContext) {
            Poo *localPoo = [poo inContext:localContext];
            [localPoo deleteEntity];
        }];
    }
    else if (editingStyle == UITableViewCellEditingStyleInsert) {
        // Create a new entity and save
    }
}
```


## The 4 Crazy NSFetchedResultsControllerDelegate methods

```objc
- (void)controllerWillChangeContent:(NSFetchedResultsController *)controller {
    [self.tableView beginUpdates];
}

- (void)controller:(NSFetchedResultsController *)controller didChangeSection:(id )sectionInfo
           atIndex:(NSUInteger)sectionIndex forChangeType:(NSFetchedResultsChangeType)type {

    switch(type) {
        case NSFetchedResultsChangeInsert:
            [self.tableView insertSections:[NSIndexSet indexSetWithIndex:sectionIndex]
                          withRowAnimation:UITableViewRowAnimationFade];
            break;

        case NSFetchedResultsChangeDelete:
            [self.tableView deleteSections:[NSIndexSet indexSetWithIndex:sectionIndex]
                          withRowAnimation:UITableViewRowAnimationFade];
            break;
    }
}

- (void)controller:(NSFetchedResultsController *)controller didChangeObject:(id)anObject
       atIndexPath:(NSIndexPath *)indexPath forChangeType:(NSFetchedResultsChangeType)type
      newIndexPath:(NSIndexPath *)newIndexPath {

    UITableView *tableView = self.tableView;

    switch(type) {

        case NSFetchedResultsChangeInsert:
            [tableView insertRowsAtIndexPaths:[NSArray arrayWithObject:newIndexPath]
                             withRowAnimation:UITableViewRowAnimationFade];
            break;

        case NSFetchedResultsChangeDelete:
            [tableView deleteRowsAtIndexPaths:[NSArray arrayWithObject:indexPath]
                             withRowAnimation:UITableViewRowAnimationFade];
            break;

        case NSFetchedResultsChangeUpdate:
            [self configureCell:[tableView cellForRowAtIndexPath:indexPath]
                    atIndexPath:indexPath];
            break;

        case NSFetchedResultsChangeMove:
            [tableView deleteRowsAtIndexPaths:[NSArray arrayWithObject:indexPath]
                             withRowAnimation:UITableViewRowAnimationFade];
            [tableView insertRowsAtIndexPaths:[NSArray arrayWithObject:newIndexPath]
                             withRowAnimation:UITableViewRowAnimationFade];
            break;
    }
}

- (void)controllerDidChangeContent:(NSFetchedResultsController *)controller {
    [self.tableView endUpdates];
}
```

