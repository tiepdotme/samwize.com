---
layout: post
title: "Deep Copying of NSArray/NSDictionary/Any Object"
date: 2014-05-12 13:25:16 +0800
comments: true
categories: [iOS]
---

When you use `[array copy]` or `[array mutableCopy]`, you expect to get a complete new copy of the array/dictionary.

However, it doesn't work that way.

Doing so will only gives you a [*shallow copy*](https://developer.apple.com/library/mac/documentation/cocoa/conceptual/Collections/Articles/Copying.html#//apple_ref/doc/uid/TP40010162-SW3). What you usually want is a *deep copy* - that is to copy every object nested within.

<!-- more -->

## Level 1 copies ##

If your array has only 1 level (it is not nested), then you can use this:

```objc
NSArray *deepCopyArray=[[NSArray alloc] initWithArray:someArray copyItems:YES];
```


## Nested levels copies ##

But if you have an array nested in array.. then you need *true deep copy*, as Apple mentioned:

```objc
NSArray* trueDeepCopyArray = [NSKeyedUnarchiver unarchiveObjectWithData:[NSKeyedArchiver archivedDataWithRootObject:oldArray]];
```

`NSKeyedArchiver` will tranverse and archive all objects (which must conform to `<NSCoding`), then later unarchive to get a new copy.


## NSCoding ##

If you have custom objects/models in the array, then you need to implement the protocol `<NSCoding>` (not NSCopying).

Note: `NSArray`, `NSDictionary` and other primitive types are already NSCoding-compliant.

There are 2 methods that you need to implement, which are just boring-tedious-code to declare which properties to be encoded, and later decoded and init with.

```objc
- (void)encodeWithCoder:(NSCoder *)coder
{
    [coder encodeObject:self.foo forKey:@"foo"];
    [coder encodeObject:self.poo forKey:@"poo"];
    ...
}

- (instancetype)initWithCoder:(NSCoder *)coder
{
    self = [super init];
    if (self) {
        self.foo     = [coder decodeObjectForKey:@"foo"];
        self.poo     = [coder decodeObjectForKey:@"poo"];
        ...
    }
    return self;
}
```


