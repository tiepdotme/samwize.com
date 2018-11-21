---
layout: post
title: "Creating a Custom NSView With Xib"
date: 2018-11-21T15:42:42+08:00
categories: [macOS]
---

I would have expected the process to be similar to iOS equivalence (`UIView`), but nope.

Yet I am no longer surprised :D

## The Swift Code

To load a view from the nib file require some code, and a further step to "copy" the subviews and auto layout constraints, which I lifted from [here](https://stackoverflow.com/a/51350799/242682).

```swift
class MyView: NSView {

    @IBOutlet var contentView: NSView!

    override init(frame frameRect: NSRect) {
        super.init(frame: frameRect)
        setup()
    }

    required init?(coder decoder: NSCoder) {
        super.init(coder: decoder)
        setup()
    }

    private func setup() {
        let bundle = Bundle(for: type(of: self))
        let nib = NSNib(nibNamed: .init(String(describing: type(of: self))), bundle: bundle)!
        nib.instantiate(withOwner: self, topLevelObjects: nil)

        contentView.subviews.forEach { addSubview($0) }

        for constraint in contentView.constraints {
            let firstItem = (constraint.firstItem as? NSView == contentView) ? self : constraint.firstItem
            let secondItem = (constraint.secondItem as? NSView == contentView) ? self : constraint.secondItem
            addConstraint(NSLayoutConstraint(item: firstItem as Any, attribute: constraint.firstAttribute,
                                             relatedBy: constraint.relation, toItem: secondItem, attribute: constraint.secondAttribute,
                                             multiplier: constraint.multiplier, constant: constraint.constant))
        }
    }

}
```

## Setting up the xib

1. Create a xib file named the same, that is `MyView.xib`
2. Set File's Owner > Class > `MyView`
3. Connect the root view to the `contentView` IBOutlet

With that, you can create the view programmatically with `MyView()` or via Interface Builder.
