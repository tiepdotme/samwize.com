---
layout: post
title: "Creating a Custom NSView With Xib"
date: 2018-11-21T15:42:42+08:00
categories: [macOS]
---

I would have expected the process to be similar to iOS equivalence (`UIView`), but nope.

Yet I am no longer surprised :D

## The Swift Code

To load a view from the nib file requires some code to:

1. Load nib from bundle and instantiate
2. Add the root view (contentView) to the custom view
3. Create constraints to fill the root view (the code uses [cartography](https://github.com/robb/Cartography))

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

        addSubview(contentView)
        constrain(self, contentView) { view, subview in
            subview.edges == view.edges
        }
    }

}
```

## Setting up the xib

1. Create a xib file named the same, that is `MyView.xib`
2. Set File's Owner > Class > `MyView`
3. Connect the root view to the `contentView` IBOutlet

With that, you can create the view programmatically with `MyView()` or via Interface Builder.
