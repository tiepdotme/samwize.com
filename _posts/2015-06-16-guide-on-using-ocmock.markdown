---
layout: post
title: "Guide on using OCMock"
date: 2015-06-16 22:31:33 +0800
comments: true
categories: [iOS]
---

[OCMock](http://ocmock.org) is a great framework for Objective-C (iOS or Mac OS) for test driven development.

It changes the behaviours of classes during testing.

This is a guide on how to use OCMock, and also raise some of the pitfalls.

<!-- more -->

## 1.  Mock an instance method

If you have a class `MyClass`, an instance `anObject`, and an instance method `foo`, you can change the return value of `foo` during testing:

```objc
// Create a mock for the class
id myClassMock = OCMClassMock([MyClass class]);

// Change the method's behaviour for MyClass with a stub
OCMStub([myClassMock foo]).andReturn(@"bar");

// If you only want to stub for a specific value eg Force to return Apple for Green fruit
OCMStub([myClassMock fruitWithColor:@"Green"]).andReturn(@"Apple");

// If you want to return Apple for all fruits
OCMStub([myClassMock fruitWithColor:[OCMArg any]]).andReturn(@"Apple");
```


## 2. Mock a class method

Mocking a class method is exactly the same as mocking for instance method.

```objc
id myClassMock = OCMClassMock([MyClass class]);
OCMStub([myClassMock classMethod]).andReturn(@"bar");
```


## 3. Mock an object (Partial mock)

If you have the instance `anObject`, you can use `OCMPartialMock` to create the mock. 

This mock in effect is the same as mocking the class.

```objc
id partialMock = OCMPartialMock(anObject);
OCMStub([partialMock foo]).andReturn(@"bar");
```

Pitfall: You might assume partial mock applies to only that instance `anObject`. That's wrong. If you have `anotherObject2`, it will be affected by whatever was mocked because partial mock actually mock the whole class.

```objc
[anObject foo];     // Will return @"bar"
[anObject2 foo];    // Will return @"bar" too!
```


## 4. Stop mocking

You can stop mocking to return to the real object and reset it's behaviours.

```objc
[anObject foo];     // Will return @"bar"
[anObject stopMocking];
[anObject2 foo];    // Will return as per normal
```

We use the example of partial mock, but you could call `[MyClass stopMocking]` and it will have the same effects.


## 5. Stub methods and chaining

You can stub the methods with different actions: return object, return native values, throw, post notification, forward to real object or do nothing.

```objc
OCMStub([mock someMethod]).andReturn(anObject);
OCMStub([mock aMethodReturningABoolean]).andReturn(YES);
OCMStub([mock someMethod]).andThrow(anException);
OCMStub([mock someMethod]).andPost(aNotification);
OCMStub([mock someMethod]).andForwardToRealObject();
OCMStub([mock someMethod]).andDo(anInvocation);
```

You can also [chain](http://ocmock.org/reference/#stubing-methods) `andPost` and `andReturn` with other actions.

```objc
OCMStub([mock someMethod]).andPost(aNotification).andReturn(aValue);
```


## Limitations - Especially Core Data

Not only those [listed](http://ocmock.org/reference/#limitations) on OCMock,
but there are other limitations.

The most severe is that you cannot mock a `NSManagedObject`.

There is a way to [get around](http://hackazach.net/code/2014/04/13/ios-testing-and-core-data/) it, but it is better that you never, never mock Core Data.

