---
layout: post
title: "Reference Cycle for Closures"
date: 2016-08-05T11:44:31+08:00
categories: [Swift]
---

You have probably learn about the [imperils](http://krakendev.io/blog/weak-and-unowned-references-in-swift) of reference/retain cycle.

A reference cycle occurs when object A has a strong reference to object B, and vice versa.

This happens not only between class instances, but also **between class instance and closure**.


## Are we superfluously capturing self as weak/unowned?

We often see codes littered with `[weak self]` or `[unowned self]` in the capture list of closures. 

But are they necessary? Do we ALWAYS have to use unowned/weak inside closure?

These is one question that we will attempt to answer (at the end). 


## The exact problem to avoid

A reference cycle will occur if and only if:

1. Class instance has a strong reference to closure
2. Closure has a strong reference to class instance

If either (1) or (2) has a weak reference instead, then you do NOT have the problem.

Let's look closer at 2 scenarios.


## 1. Class instance with strong reference to closure

This is the typical scenario where the class instance has a strong reference to the closure directly, or indirectly.

What is mean by indirect? The instance could be holding the reference indirectly, via a third-party object.

Let's look at an example:

```swift
// Now, myStrongClass has strong reference to the closure
class MyStrongClass {
    deinit { print("deinit MyStrongClass") }
    var theClosure: (()->Void)?
    
    func runClosure() {
        theClosure = {
            dispatch_after(dispatch_time(DISPATCH_TIME_NOW, Int64(3 * Double(NSEC_PER_SEC))), dispatch_get_main_queue()) {
                print("Running the closure and this is self: \(self)")
            }
        }
        theClosure!()
    }
}

var myStrongClass: MyStrongClass? = MyStrongClass()
myStrongClass?.runClosure()
myStrongClass = nil
```

In the code above, `myStrongClass` will not deinit even when it is set to `nil`.

This is because the closure is (by default), capturing `self`/`myStrongClass` **strongly**.

The typical solution is to capture `self` with `unowned` or `weak`.


```swift
theClosure = { [weak self] in
  ...
}
```

This is the scenario where it is **necessary** to use `weak`/`unowned`, if not you will have memory leak problem.


## 2. Closure has a strong reference to class instance

In this scenario, you might think the closure can be declared with `weak` (while still capture `self` strongly):

```swift
weak var theClosure: (()->Void)?
```

But that is not possible.

`weak` can only be applied to class or class-bound protocol.

To illustrate, we'll use a closure in a local scope (therefore not strongly referenced by `self`).

```swift
class MyHolder {
    var value = "initial"
    deinit { print("deinit MyHolder") }
    
    func runClosure() {
        let strongClosure: (()->Void) = {
            dispatch_after(dispatch_time(DISPATCH_TIME_NOW, Int64(3 * Double(NSEC_PER_SEC))), dispatch_get_main_queue()) {
                print("Value of MyHolder: \(self.value)")
            }
        }
        strongClosure()
    }
}

var holder: MyHolder? = MyHolder()
holder?.value = "changed"
holder?.runClosure()       // Prints changed then deinit
holder = nil

// Result: holder will deinit once the closure has completed (and printed the value)
// closure deinit, therefore instance deinit too
```

The closure is never referenced in the object instance. It is used only in the local scope of `runClosure`.

`strongClosure` captures self strongly. That's why it will print the value first, then self will be released.

The point to emphasize here is:

**If `self` does NOT have a strong reference to the closure, it is okay to have the closure capture `self` strongly.**


## The Case of Singleton

It is more common that you have scenario (2) in the case of using a singleton:

```swift
class Singleton {
    static let sharedInstance = Singleton()    
    func runClosure(closure: (()->Void)) {
        dispatch_after(dispatch_time(DISPATCH_TIME_NOW, Int64(3 * Double(NSEC_PER_SEC))), dispatch_get_main_queue()) {
            closure()
        }
    }
}

class MyHolder {
    ...
    func runClosure() {
        Singleton.sharedInstance.runClosure { 
            print("Value of MyHolderWithSingleton: \(self.value)")
        }
    }
}
```

The above code is similar to (2) -- the closure has a strong reference to self, but self does NOT have strong reference tot he closure, nor to the singleton.

I have seen many codes that **superfluously** capture self as `weak` like this:

```swift
func runClosure() {
    Singleton.sharedInstance.runClosure { [weak self] in
        print("Value of MyHolderWithSingleton: \(self?.value)")
    }
}
```

Usually `self` is a view controller, and the superfluous code above have the closure reference the view controller weakly. 

BUT, `self` is not referencing the closure strongly in the first place!

There is no need to avoid a reference cycle, because there isn't one in the first place.

Once again:

**If self does NOT have a strong reference to the closure, it is okay to have the closure capture self strongly.**