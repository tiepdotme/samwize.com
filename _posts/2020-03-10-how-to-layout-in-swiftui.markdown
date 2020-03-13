---
layout: post
title: "How to Layout in SwiftUI?"
date: 2020-03-10T17:34:07+08:00
categories: [SwiftUI]
---

SwiftUI changes everything, again.

Auto Layout is the past. With SwiftUI, we have these tools at our disposal:

1. Alignment Guides
2. Geometry Reader
3. Anchor Preferences

## How layout works?

Watch [WWDC 2019 advanced composition and layout](https://developer.apple.com/videos/play/wwdc2019/237/) for a good introduction of the new layout system.

The process is basically:

1. Parent proposes a size for child
2. **Child chooses its own size**
3. Parent places child in parent's coordinate space

SwiftUI is very different because it is the child who defines it's own size! It is **modern parenting** (compared to olden days, UIKit is more "traditional").

The children can even be rebellious -- choosing a size bigger than proposed, and positioning itself out of the parent! There is nothing the parent can do about that..

## No more auto layout constraints

In place of constraints, you have view modifiers like `aspectRatio`, `padding`, `frame`, `offset` etc..

This will feel unnatural. Constraints like equal width is now difficult to achieve. There are lots of trade off, but still cool.

We now look at the 3 new concepts to handle layout.

## 1. Alignment Guides

The layout system aligns multiple views by positioning all of them along an imaginary line (the guide).

To do that, you need to specify a `CGFloat` value for your view, for a particular alignment. The built-in alignments are `top`, `trailing`, `center`, etc that we know.

We can have custom alignments too. This is a boilerplate to creating one:

```swift
// Create a custom vertical alignment
extension VerticalAlignment {
    private enum SomeVerticalAlignment : AlignmentID {
        static func defaultValue(in d: ViewDimensions) -> CGFloat {
            return d[VerticalAlignment.center]
        }
    }
    static let someVerticalAlignment = VerticalAlignment(SomeVerticalAlignment.self)
}
```

Then use it in your views.

```swift
HStack(alignment: .someVerticalAlignment) {
  Text("Center +10")
    .alignmentGuide(.someVerticalAlignment) { $0[.center] + 10 }
  Text("Center -10")
    .alignmentGuide(.someVerticalAlignment) { $0[.center] - 10 }
}
```

The `HStack` says it is using the custom alignment, and the 2 texts specify where that alignment is for their view.

The best resource out there is from [The SwiftUI Lab](https://swiftui-lab.com/alignment-guides/). Read it for more info.

## 2. Geometry Reader

Might come as a surprise, but [Geometry Reader](https://developer.apple.com/documentation/swiftui/geometryreader) is also a view!

> A container view that defines its content as a function of its own size and coordinate space.

Using `GeometryReader`, children can know the size and position of their parent (the container).

```swift
GeometryReader { geometry in
  VStack {
    // The children can now use the size, position etc
    Color.yellow
      .frame(width: geometry.size.width * 0.3, height: 40)
  }
}
```

## 3. Anchor Preferences

**Preferences** is like a contract that you can pass between views.

> Access view preferences and provide child views with configuration data.

**Anchor Preferences** is a special kind of preferences, useful for setting & retrieving geometry from child views.

While Geometry Reader is for child to know their parent, Preference is the other way round - for parent (and anyone) to know children.

It require a 4 steps to use:

```swift
// 1. Define a data for holding the preference
struct MyAnchorPreferenceData {
    let bounds: Anchor<CGRect> // It can also be some kind of CGPoint data
}

// 2. Define a preference key
struct MyAnchorPreferenceKey: PreferenceKey {
    static var defaultValue: [MyAnchorPreferenceData] = []
    static func reduce(value: inout [MyAnchorPreferenceData], nextValue: () -> [MyAnchorPreferenceData]) {
        value.append(contentsOf: nextValue())
    }
}

// 3. Child: Set anchor preference
// `value` is the bounds (CGRect) for the view. It can also be top, center, etc (CGPoint)
// `transform` will create the preference, and $0 will be a CGRect/CGPoint as per `value` type
childView.anchorPreference(key: MyAnchorPreferenceKey.self, value: .bounds, transform: { [MyAnchorPreferenceData(bounds: $0)] })

// 4. Parent: Retrieve anchor preference
parentView.backgroundPreferenceValue(MyAnchorPreferenceKey.self) { preferences in
    return GeometryReader { geometry in
        // Return a view as a background
        // Use geometry and preferences like this to get the child bounds:
        let preference = preferences.first! // preferences is [MyAnchorPreferenceData], and you should filter to your needs
        let bounds = geometry[preference.bounds]
        ...
    }
}
```

Note that the `bounds` will automatically be in the parent's `CoordinateSpace`. Usually, `GeometryReader` has to work with a `CoordinateSpace` (where you can name one with "string", and that's ugly). By using Anchor Preferences with Geometry Reader, we can avoid the `CoordinateSpace`.

The parent can also use `.overlayPreferenceValue()`, which will draw in front instead.

Again, SwiftUI Lab has good write up on the topic:

- [Preferences](https://swiftui-lab.com/communicating-with-the-view-tree-part-1/)
- [Anchor Preferences](https://swiftui-lab.com/communicating-with-the-view-tree-part-2/)
- [Simplifying the Use of View Preferences](https://swiftui-lab.com/view-extensions-for-better-code-readability/) to refactor code (using View extension).

## PITFALL: VerticalAlignment or HorizontalAlignment

Most of the time, you can be lazy and just use `.center`. But in some cases, it could have undesirable effect.

```swift
HStack(alignment: .someVerticalAlignment) {}
  SomeView().alignmentGuide(.someVerticalAlignment) {
    $0[.center] // Sometimes, this might be inferred as HorizontalAlignment.center
    $0[VerticalAlignment.center] // This will be what we want
  }
}
```
