---
layout: post
title: "How to handle keyboard hide/show events in your view"
date: 2015-04-29 09:44:36 +0800
comments: true
categories: [iOS]
---

It is a very common task in iOS when you have eg `UITextField`, which on tap brings up the keyboard, and in the process obscure part of your view.

Unfortuantely, iOS leaves this situation to the developer to handle.

If you have a simple form for users to enter some text, then I suggest using [IQKeyboardManager](https://github.com/hackiftekhar/IQKeyboardManager) to this common problem.

<!-- more -->

## IQKeyboardManager

This [open soure library by hackiftekhar](https://github.com/hackiftekhar/IQKeyboardManager) is a terrific component for iOS.

Just install (eg via Cocoapods), and that's it.

Zero code needed.

How the library achieves this is by having it's manager listening to the standard `UIKeyboardWillShowNotification` during app initialization, and then adjusting the whole window view when necessary.

It's a shame iOS didn't ship with a capability like that.


## How to manually handle the keyboard

While IQKeyboardManager is great, sometimes you want to manage these keyboard events manually.

For example, if you have a `UITextView` for entering paragraph of text, and you want only the insets to change when keyboard is showing/hiding, then IQKeyboardManager isn't configured for that.

Or for example, if you have a toolbar that you want to stick just above the keyboard, then IQKeyboardManager isn't suited for that.

This is the code to manually handle, and with proper animations that synchronize with keyboard animation:

```objc
- (void)viewDidLoad {
    [super viewDidLoad];

    // Listen to the standard keyboard notifications
    [[NSNotificationCenter defaultCenter] addObserver:self
                                             selector:@selector(keyboardWillShow:)
                                                 name:UIKeyboardWillShowNotification object:nil];
    
    [[NSNotificationCenter defaultCenter] addObserver:self
                                             selector:@selector(keyboardWillHide:)
                                                 name:UIKeyboardWillHideNotification object:nil];
}

- (void)dealloc {
    [[NSNotificationCenter defaultCenter] removeObserver:self
                                                    name:UIKeyboardDidShowNotification
                                                  object:nil];
    [[NSNotificationCenter defaultCenter] removeObserver:self
                                                    name:UIKeyboardWillHideNotification
                                                  object:nil];
}

- (void)keyboardWillShow:(NSNotification*)aNotification {    
    [self adjustForKeyboard:aNotification showing:YES];
}

- (void)keyboardWillHide:(NSNotification*)aNotification {
    [self adjustForKeyboard:aNotification showing:NO];
}

- (void)adjustForKeyboard:(NSNotification*)aNotification showing:(BOOL)showing {
    NSDictionary* info = [aNotification userInfo];
    CGRect keyboardFrame = [[info objectForKey:UIKeyboardFrameEndUserInfoKey] CGRectValue];
    CGRect convertedFrame = [self.view convertRect:keyboardFrame fromView:self.view.window];
    CGSize kbSize = convertedFrame.size;
    
    // 1. Adjust for the UITextView
    UIEdgeInsets newInsets;
    // If keyboard showing, add an inset
    if (showing) {
        newInsets = UIEdgeInsetsMake(0.0, 0.0, kbSize.height, 0.0);
    } else {
    // Else keyboard hiding, rest the inset
        newInsets = UIEdgeInsetsMake(0, 0, 0, 0);
    }
    _textView.contentInset = newInsets;
    _textView.scrollIndicatorInsets = newInsets;
    
    // 2. Adjust toolbar with animation
    // Get keyboard animation info
    NSTimeInterval animationDuration;
    UIViewAnimationCurve animationCurve;
    [[info objectForKey:UIKeyboardAnimationCurveUserInfoKey] getValue:&animationCurve];
    [[info objectForKey:UIKeyboardAnimationDurationUserInfoKey] getValue:&animationDuration];
    // A simple convertion of UIViewAnimationCurve to UIViewAnimationOptions
    UIViewAnimationOptions animationOption = animationCurve << 16;
    if (animationDuration == 0)
        animationDuration = 0.1;
    
    // Start animation
    [self.view layoutIfNeeded];
    [UIView animateWithDuration:animationDuration
                          delay:0
                        options:animationOption
                     animations:^{
                         
                         // Adjust for toolbar
                         if (showing)
                             _toolbarBottomSpace.constant = kbSize.height;
                         else
                             _toolbarBottomSpace.constant = 0;
                         [self.view layoutIfNeeded];
                         
                     } completion:nil];
}
```

In `viewDidLoad`, it is where we add the view controller as the observer to the keyboard events. Subsequently in `dealloc`, remove the observer.

The gist is in `adjustForKeyboard:showing:`.

`aNotification` contains a dictionary about the keyboard size and animation details. 

A little trick is needed to convert `UIViewAnimationCurve` to `UIViewAnimationOptions`, which is needed for the new animation code.

For the toolbar, we change `_toolbarBottomSpace`, which is a vertical spacing constraint for the toolbar view to the bottom layout guide. You need to create this constraint on your storyboard (with autolayout).
