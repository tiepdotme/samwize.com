---
layout: post
title: "How to save using MagicalRecord"
date: 2014-03-29 18:21:47 +0800
comments: true
categories: [iOS]
---

I spent some time to research on the correct/recommended way to save using [MagicalRecord](https://github.com/magicalpanda/MagicalRecord), and felt a need to write this post.

It is relevant as of March 2013, using version 2.2 of MagicalRecord.

<!-- more -->

## Tutorials out there

I wrote this because even tutorials eg from [Ray Wenderlich](http://www.raywenderlich.com/56879/magicalrecord-tutorial-ios) (2014 Feb) didn't use the recommended methods.

Or tutorials eg from [Yannick Loriot](http://yannickloriot.com/2012/03/magicalrecord-how-to-make-programming-with-core-data-pleasant/) (2012 Mar) are outdated.


## Official methods

And unfortunately, the 'official document' on [saving](https://github.com/magicalpanda/MagicalRecord/blob/master/Docs/Saving.md) explains more on the _recent changes to saving_. That is least helpful.

Though if you dig further into [threading](https://github.com/magicalpanda/MagicalRecord/blob/master/Docs/Threads.md), you will find a better and safer way to save.

And that is using `[MagicalRecord saveWithBlock:]`.


## What the author says

On [StackOverflow](http://stackoverflow.com/a/15551603/242682), he said:

> don't use `[NSManagedObjectContext save:]`

On his [blog](http://saulmora.com/2013/09/15/why-contextforcurrentthread-doesn-t-work-in-magicalrecord/), he explained why.


## The code

Finally, here is the code to update an entity.

`Poo` is my `NSManagedObject`. Replace it with your own model.

```objc
    Poo *poo = [Poo MR_findFirst];
    // Update the entity in the block of saveWithBlock:
    [MagicalRecord saveWithBlock:^(NSManagedObjectContext *localContext) {
        Poo *localPoo = [poo MR_inContext:localContext];
        localPoo.someProperty = "awesome";
        // That's all to updating
    }];
```

Or if you want to delete the entity, use `[localPoo deleteEntity]` in the block.


## On threading

Note that the block will be run in a background thread, not the main thread.

So if you want to do some UI updating, you should use `saveWithBlock:completion:` method.

```objc
[MagicalRecord saveWithBlock:^(NSManagedObjectContext *localContext) {
    // This block runs in background thread
} completion:^(BOOL success, NSError *error) {
    // This block runs in main thread
}];
```

Alternatively, you can use `saveWithBlockAndWait:` method.

```objc
    [MagicalRecord saveWithBlockAndWait:^(BOOL success, NSError *error) {
        // Any updating or deletion will be on main thread
    }];
```

Hopes this helps you!

