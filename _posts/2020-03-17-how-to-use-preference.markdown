---
layout: post
title: "How to Use Preference"
date: 2020-03-17T12:12:00+08:00
categories: [SwiftUI]
---

Preference is the way to pass data **from children to parent**.

On the other hand, [environment](/2020/03/16/guide-to-environment-and-environmentobject/) passes data **from parent to children**.

## Custom Preference

The steps to creating a preference include creating a preference key (with type and default), then the children set value with the key.

```swift
/// 1. Declare a preference key
struct SomePreference: PreferenceKey {
    /// The type is CGSize here. It can be any type you want.
    static var defaultValue: CGSize { return .zero }

    /// Because multiple children can use the same key, a reduce function will pass it up to parent.
    /// This implementation will reduce to return the min size.
    static func reduce(value: inout CGSize, nextValue: () -> CGSize) {
        let nextValue = nextValue()
        let minWidth = min(value.width, nextValue.width)
        let minHeight = min(value.height, nextValue.height)
        value = CGSize(width: minWidth, height: minHeight)
    }
}

struct SomeView: View {
    var body: some View {
        VStack {
            /// 2. Children will set it
            GeometryReader() { g in
                Text("I gonna pass my size to my parent")
                    .preference(key: SomePreference.self, value: g.size)
            }
            GeometryReader() { g in
                Text("Me too!")
                    .preference(key: SomePreference.self, value: g.size)
            }
        }
        .onPreferenceChange(SomePreference.self) { width in
            // 3. Parent receives it
            print("Preference changed: \(width)")
        }
    }
}
```

NOTE: The `reduce` function is required because **multiple children gonna pass it to a parent**. The function is to reduce to a single value.

But if you don't want to reduce, you could read the next section.

## Collection Preference

If you don't want to reduce, then a way to pass every children data is to make the type an array, then simply append values to it.

```swift
struct CollectionPreference: PreferenceKey {
    static var defaultValue: [CGSize] { return [] }
    static func reduce(value: inout [CGSize], nextValue: () -> [CGSize]) {
        value.append(contentsOf: nextValue())
    }
}
```
