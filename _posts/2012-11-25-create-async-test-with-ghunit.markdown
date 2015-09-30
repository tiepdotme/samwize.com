---
layout: post
title: "Create Async Test with GHUnit"
date: 2012-11-25 01:44
comments: true
categories: iOS, Testing
---

One of the [greatest](/2012/10/03/sentestingkit-does-not-support-wait-for-blocks) feature with GHUnit is that it supports unit tests that perform asynchronous stuff. 

The [ExampleAsyncTest](http://gabriel.github.com/gh-unit/docs/appledoc_include/guide_testing.html) from GHUnit documentation provides a glimpse of what you need to do. However, it still lacks warning you on the [common pitfalls](http://stackoverflow.com/questions/7613761/why-does-a-false-assertion-in-async-test-in-ghunit-crash-the-app-instead-of-just).

This is step by step of how you can use GHUnit async test:

<!-- more -->

## Subclass GHAsyncTestCase instead ##

Change the interface class to use `GHAsyncTestCase`.

``` objc
    @interface ExampleAsyncTest : GHAsyncTestCase { }
    @end
```

## Prepare and wait ##

Setup the test method with the asynchronous/block call like this:

``` objc
- (void)testAsync {
    // Prepare asynchronous action
    [self prepare];
    
    // The method call with block
    static NSArray *_objects;
    [Query findObjectsInBackgroundWithBlock:^(NSArray *objects, NSError *error) {
        // IMPORTANT: Do not call any assert here
        // If you want, do it after waitForStatus
        // GHAssertEquals(...) will crash!

        // Instead, we assign to a static var for testing later
        _objects = objects;

        // Notify that block method has completed
        [self notify:kGHUnitWaitStatusSuccess forSelector:@selector(testObjectRelationships)];
    }];
    
    // Wait for block to finish, or timeout
    [self waitForStatus:kGHUnitWaitStatusSuccess timeout:10.0];

    // Do asserts here instead
    GHAssertEquals(99U, [_objects count], nil);
}
```

## Pitfall: Assert in block ##

It is [common](http://stackoverflow.com/questions/7613761/why-does-a-false-assertion-in-async-test-in-ghunit-crash-the-app-instead-of-just) that you assert within the block. 

But that will just give the following exception when the test fails.

    Terminating app due to uncaught exception 'GHTestFailureException', reason: ...

To make it work, you don't call any asserts within the block.

Leave it until `waitForStatus` is unblocked.
