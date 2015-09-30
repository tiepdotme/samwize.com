---
layout: post
title: "First taste with visual language format"
date: 2014-05-13 10:19:38 +0800
comments: true
categories: [iOS]
---

As much as possible, I use storyboard with autolayout, and create constraints from there.

But when you are required to create controls dynamically, that is not possible.

You have to fall back to coding the controls and writing constraints in code. There are generally 2 ways to create constraints in code:

1. writing each constraint directly, or

2. writing Visual Language Format (VLF).

<!-- more -->

## What is Visual Language Format

VLF is a string describing 1 or more constraints. It is more convenient than writing each constraint directly since it is more succinct.

An example:

```objc
[container addConstraints:[NSLayoutConstraint constraintsWithVisualFormat:@"|-[label]-|"
                                                                  options:0
                                                                  metrics:nil
                                                                    views:NSDictionaryOfVariableBindings(label)]];
```

The above will add 2 constraints - pin the left and right of label to its superview (container in this case).


## How to create views with Visual Language Format

There are some standards steps when using VLF.

This is a simple example of 2 labels in a container view.

```objc
// 1. Create the controls
// And setup other stuff eg text, font
UILabel *label1 = [[UILabel alloc] init];
UILabel *label2 = [[UILabel alloc] init];

// 2. Must set to NO
label1.translatesAutoresizingMaskIntoConstraints = NO;
label2.translatesAutoresizingMaskIntoConstraints = NO;

// 3. Add to superview (container)
[container addSubview:label1];
[container addSubview:label2];

// 4. Use the macro to create a dictionary of views for convenience
NSDictionary *views = NSDictionaryOfVariableBindings(label1, label2);

// 5. Add horizontal constraint to superview
[container addConstraints:[NSLayoutConstraint constraintsWithVisualFormat:@"|-2-[label1]-2-|"
                                                                  options:0
                                                                  metrics:nil
                                                                    views:views]];
// 6. Add vertical constraint to superview
[container addConstraints:[NSLayoutConstraint constraintsWithVisualFormat:@"V:|-2-[label1(40)]-2-[label2]]"
                                                                  options:NSLayoutFormatAlignAllCenterX
                                                                  metrics:nil
                                                                    views:views]];
```

The gist is in adding the 2 constraints to container. You will need to add once for horizontal, and once for vertical.

`|-2-[label1]-2-|` means superview with left padding of 2 to label1, then with right padding of 2 to superview.

`V:|-2-[label1(40)]-2-[label2]]` means a vertical constraint, with top padding of 2 from superview to label1, which has height 40, then padding of 2 to label2.

For `options` in vertical constraint, I also added a `NSLayoutFormatAlignAllCenterX`, which means all those in `views` (label1 and label2, but NOT container), will all align center x.

If you need to align label2/label2 to container, you have to write the constraint directly. Some things are not possible with VLF. There is no syntax to describe the alignment.

For more information on the syntax, you can refer to [Apple Doc](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/AutolayoutPG/VisualFormatLanguage/VisualFormatLanguage.html). [Command Shift](http://commandshift.co.uk/blog/2013/01/31/visual-format-language-for-autolayout/) also has a comprehensive guide on the
