---
layout: post
title: "Pitfall: Creating Parent/Abstract Entitiy in Core Data"
date: 2015-06-02 11:05:38 +0800
comments: true
categories: [iOS]
---

If you design your data model with parent entity, then you are most likely doing it wrong, unknowingly.

Because when you do so, Core Data will create 1 common table for ALL the children entities!

This is bad design (on Core Data part) and it will slow down the performance.

Read more about this under the section [Entity Hierarchy vs. Class Hierarchy](http://www.objc.io/issue-4/core-data-models-and-model-objects.html).

<!-- more -->

## Designing a flat Entity Hierarchy

When you have common attributes (such as `createdAt`, `updatedAt`) between entities, most likely you only want to enjoy the convenient of using them in your code.

Steps:

- Copy and paste the common attributes across entities
- Create a parent `NSManagedObject` that declare the common attributes
- Subclass with the parent `NSManagedObject`


## Using mogenerator

When using mogenerator, the class files are auto generated, and it is generated according to the schema design. 

This complicate things..

To fix:

- Create the parent entity with the common attributes
- Generate
- In all the `_Entity.h`, subclass with the parent class instead of `NSManagedObject`, and remove codes on the common attributes
- Remove `objectID` in the parenty entity class (because each child has the same attribute)

Tedious workaround, but I don't have a better way to work with mogenerator. 

If you know, let me know :)

