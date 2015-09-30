---
layout: post
title: "Step-by-step Guide to using MagicalRecord and mogenerator"
date: 2014-03-27 16:55:30 +0800
comments: true
categories: iOS
---

Core Data remains a nightmare framework, with many lines of code to get started.

Apple provides a [snippet section just for Core Data](https://developer.apple.com/library/mac/documentation/DataManagement/Conceptual/CoreDataSnippets/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008282-SW1). Browse through it, be afraid, then appreciate the 2 libraries I will be introducing here.

[MagicalRecord](https://github.com/magicalpanda/MagicalRecord) and [mogenerator](https://github.com/rentzsch/mogenerator) are 2 popular libraries to use when dealing with Core Data. NSHipster mentioned [others](http://nshipster.com/core-data-libraries-and-utilities/), but these 2 are the _must use_.

In this post, I will explain a step by step to set them up, for there is a lack of proper documentation.

<!-- more -->

## 1. Install MagicalRecords

Using CocoaPods, add this to your podfile:

    pod 'MagicalRecord/Shorthand', '~> 2.2'

Then `pod install`.

Note: We want to used shorthand, therefore the additional "/Shorthand" in the podfile. Shorthand is recommended, so that you use `findAll` instead of `MR_findAll`.


## 2. Setup pch

Your pch file should look like this:

```objc
#ifdef __OBJC__
    #define MR_SHORTHAND
    #import <CoreData+MagicalRecord.h>
#endif
```


## 3. Setup when application load/exit

```objc
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    [MagicalRecord setupCoreDataStack];
}

- (void)applicationWillTerminate:(UIApplication *)application {
    [MagicalRecord cleanUp];
}
```

Or if you want to setup with a different sqlite name, or deal with migration, refer to this [documentation](https://github.com/magicalpanda/MagicalRecord/blob/master/Docs/GettingStarted.md).


## 4. Using MagicalRecord API

[MagicalRecord](https://github.com/magicalpanda/MagicalRecord) documentation is kind of messy. Here's a orderly guide to using:

- [Basic & Advanced fetching](https://github.com/magicalpanda/MagicalRecord/blob/master/Docs/Fetching.md)
- [Saving](https://github.com/magicalpanda/MagicalRecord/blob/master/Docs/Saving.md)
- [Threading](https://github.com/magicalpanda/MagicalRecord/blob/master/Docs/Threads.md) make easy
- [Logging](https://github.com/magicalpanda/MagicalRecord/blob/master/Docs/Logging.md) which you can disable
- [iCloud support](https://github.com/magicalpanda/MagicalRecord/blob/master/Docs/iCloud.md)

At this point, you may already use MagicalRecord API with Core Data. Just create your model and entities. Refer to a [tutorial](http://yannickloriot.com/2012/03/magicalrecord-how-to-make-programming-with-core-data-pleasant/) if needed.

But it is better to use mogenerator to generate the entitiy classes. I will explain setting up of mogenerator in the rest of the sections. But if you are not using mogenerator, stop here.


## 5. Install mogenerator

The value of mogenerator is:

> Generate 2 classes per entity - 1 for machine and 1 for human (which you can edit)

It is a command-line tool, which you can install using brew:

    // On your command line
    $ sudo brew install mogenerator
    // Or if you need to update
    $ sudo brew update && brew upgrade mogenerator


## 6. Add a Model

Next, add a core data model (if you have not).

Go to File > New File > Core Data > Data Model

You should save `Model.xcdatamodeld` in a `Models` directory.


## 7. Add new Entity

Add a new entity into the model. Assume the entity name is `Person`.

Change the entity name, [and also class name](http://stackoverflow.com/questions/14183895/mogenerator-error-skipping-entity-myobjectname-nsmanagedobject-because-it-doe) to `Person` too.



## 8. Setup target and build script

Follow the guide to setup a [target for mogenerator](http://raptureinvenice.com/getting-started-with-mogenerator/). That creates a mogenerator target that you can build everytime you change your model.

Here's my build script:

    mogenerator -m "MyApp/Models/Model.xcdatamodeld/Model.xcdatamodel" -O MyApp/Models --template-var arc=true

`MyApp/Models/` is the directory containing all my models related file, which is also where I save the data model when I created it. You may make changes accordingly.

You can also refer to the [command-line options](http://stackoverflow.com/questions/3589247/how-do-the-mogenerator-parameters-work-which-can-i-send-via-xcode), if you need to change how it generates.

_UPDATE: If you are using v1.28 (Sep 2014), and wants to use ARC, the shorter command is:_

    mogenerator --v2 -m "MyApp/Models/Model.xcdatamodeld" -O "Jade/Models"

Note that if you have a new version of your model, you should edit the build script correspondingly. eg. I have a "Model 2.xcdatamodel":

    mogenerator -m "MyApp/Models/Model.xcdatamodeld/Model 2.xcdatamodel" -O MyApp/Models --template-var arc=true


## 9. Build mogenerator target

Select the mogenerator target and build. This will generate all the files.

`Person.h/m` is the file for human, which you may edit.

`_Person.h/m` is the file for machines, which you should never touch.


## 10. Add the files to project

Add `_Person.h/m` into `Models` group in your project.

That's it!

Start using with MagicalRecords, or refer to a [tutorial](http://yannickloriot.com/2012/03/magicalrecord-how-to-make-programming-with-core-data-pleasant/) if needed.
