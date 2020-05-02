---
layout: post
title: "A Few Things About @State"
date: 2020-04-30T17:45:38+08:00
categories: [SwiftUI]
---

## @State is mutable

When you declare `@State`, we know it is a [property wrapper](/2019/12/04/guide-to-property-wrapper/) wrapping a certain value. For example:

```swift
@State var foo: Int = 1
```

It is managing an Int.

**And it may mutate the Int.**

On the other hand, if you declare without `@State`, **you cannot mutate the View's property**.

```swift
var foo: Int = 1

Button("Change foo") {
  self.foo = 2 // Error: 'self' is immutable
}
```

## How to init @State

```swift
struct MyView: View {
    @State var foo: Int = 1
    init(foo: Int) {
        self.foo = foo
    }
}
```

If you provide your own View initialization, and you init like the above, you'll be surprised. `MyView(foo: 2)` will NOT set foo to 2. It will remain as 1.

The way to init an `@State` is:

```swift
init(foo: Int) {
    self._foo = State(initialValue: foo)
}
```

Yup, you have to use an underscore `_foo` to set the state manually.

Similarly, you could need `ObservedObject(initialValue: ...)`.

## View will "refresh" on state change

Yes, we know the `body` will be called, kind of like refreshing the view, whenever the state changes.

If the state is not used within `body`, SwiftUI will be intelligent enough, and will not call upon the `body`.

## The magic between `init` and `body`

We understand children views get `init` repeatedly, whenever the state of a parent changes.

But when a child view is init, **the state will remain -- not reset to the default**. This might be obvious; because if it reset to default, it will look like a UI bug.

Yet if you think deep into what's happening, you will find it _magical_ :)

Be careful what you `init` in a view. It should be lightweight, and likely SwiftUI will frequently initialize the struct. Yet, the state, in the view struct, **will be restored** to what it was.

Or in the words of [The Stranger Thing in @State](https://nalexn.github.io/stranger-things-swiftui-state/):

> The trick here is that the view is not always connected to that state store: SwiftUI does plug it in when the view needs a redraw or receives a SwiftUI-originated callback, but plugs it out afterward.

So, it's best to think of the View and the store (`@State`, `@ObservedObject`) as disconnected pieces.

## Extending `Equatable`

You should expect SwiftUI to `init` your view _anytime_, and restore the state.

How does SwiftUI restore the state?

It will do it's [own diff-ing](https://twitter.com/jsh8080/status/1206617804113432576) to know if that's the same view.

If SwiftUI seems to fail you, you might want to extend `Equatable` for your view.

```swift
extension MyView: Equatable {
    static func == (lhs: MyView, rhs: MyView) -> Bool {
        // Example: if view model id is the same, the view is equal
        return lhs.vm.id == rhs.vm.id
    }
}
```

Read up on [`EquatableView`](https://swiftui-lab.com/equatableview/).
