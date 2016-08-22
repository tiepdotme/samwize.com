---
layout: post
title: "Swift Is a Protocol Oriented Programming Language"
date: 2016-08-11T16:58:26+08:00
categories: [Swift]
---

[WWDC 2015](https://developer.apple.com/videos/play/wwdc2015/408/) is where Protocol Oriented Programming (POP) is officially being preached.

It gave us a different perspective of programming, one that does NOT use **class and inheritance**.

Instead, POP use **struct and protocol**.


## The Problem with Object Oriented Programming Language

The problem is that in practice, using class inheritance is at times _not correct_.

Or not flexible.

Because with inheritance, the only structure you can make is a **hierarchy**. You are limited by that.

But with protocols, you are essentially giving your object traits. 

With protocol extension for the trait, you can give it a default trait.

It makes resuable code, much more resuable (:

To build a component, you just need to specify these individual traits, like how building blocks should be!


## [An Example](http://matthijshollemans.com/2015/07/22/mixins-and-traits-in-swift-2/)

We look at an example of a login view controller, with a password text field.

```swift
class LoginViewController: UIViewController {
  let passwordValidator = PasswordValidator()

  @IBAction func loginButtonPressed() {
    if passwordValidator.isPasswordValid(passwordTextField.text!) {
      ...
    }
  }
}
```

The above use a decompostion pattern, with a `PasswordValidator` object.

This is nice and clean, as we put the validation logic in an object of it's own.

Here is how you can improve it with POP, with a default implementation.

```swift
protocol ValidatesPassword {
  func isPasswordValid(password: String) -> Bool
}

extension ValidatesPassword {
  func isPasswordValid(password: String) -> Bool {
    // A default implementation
  }
}
```

The validation logic is still a separate object, but now in a protocol extension.

To use,

```swift
class LoginViewController: UIViewController, ValidatesPassword {
  @IBAction func loginButtonPressed() {
    if isPasswordValid(passwordTextField.text!) {
      ...
    }
  }
}
```

It is now even cleaner!

You can call `isPasswordValid` because of `ValidatesPassword`, the protocol which your view controller now has.
