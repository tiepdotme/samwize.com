---
layout: post
title: "Dismiss keyboard when tap outside a UITextField/UITextView"
date: 2014-03-27 10:36:27 +0800
comments: true
categories: [iOS]
---

This is another common problem which all beginners will face. Drop in a UITextField (we will omit UITextView from now on, but it's the same), start editing, and you will soon need to learn how to dismiss the keyboard.

The code to dismiss is simple:

    [textfield endEditing:YES];

The difficulty is where to put this code.

<!-- more -->

There are 2 scenarios - using a scroll view, or not.

## 1. Using a scroll view

If you are using a UIScrollView (or UITableView), then you will need to add a tap gesture recognizer to it.

I use storyboard most of the time. To add, drag a Tap Gesture Recognizer (in control library) to the UIScrollView.

In the tap gesture recognizer, control-drag the "Sent Actions" to your .m implementation file, and add a method eg. `hideKeyboard`.

```objc
    - (IBAction)hideKeyboard:(id)sender {
        [textfield endEditing:YES];
    }
```

If you are using code, and not Storyboard, then refer to [this](http://stackoverflow.com/a/4132774/242682).


## 2. Using a normal view

If you are using UIView without UIScrollView, it is easier.

You can override UIViewController touch events as such:

```objc
    - (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event {
        [textfield endEditing:YES];
    }
```

But the above will not work for UIScrollView because:

- scroll view in inside the view
- scroll view will receive the touch first, and not pass it on the view (according to [responder chain](https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/event_delivery_responder_chain/event_delivery_responder_chain.html))

A way to make that code work for UIScrollView is to disable interaction for UIScrollView. But whats the point of a scroll view that cannot be scrolled?  I guess I am just making a point of how views handle the touch events.

An advise: While the code works for normal view, I suggest to use the same solution like scroll view using tap gesture recognizer. This will make your code more coherent.


## 3. Dismiss on drag for scroll view

If you using storyboarding, select the scroll view. In the keyboard property, select "Dismiss on drag".

This will dismiss when you drag/scroll the scroll view.



## A note on cancelsTouchesInView

In using tap gesture recognizer, you might want to disable `cancelsTouchesInView` (set to NO), so that it pass the touch event to other views (eg table view cell).

The default is YES, which means touch event will be consumed and not passed on.

This is a checkbox if you are using storyboard.

