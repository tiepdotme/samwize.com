---
layout: post
title: "How to use Environment and EnvironmentObject"
date: 2020-03-16T16:22:38+08:00
categories: [SwiftUI]
---

Environment is a very powerful way to **pass data from parent down to children**.

It is like a [global singleton](https://www.youtube.com/watch?v=CyQ59ZfT5E0).

Don't cringe, because that's the direction SwiftUI is taking. So embrace it.

## 1. Custom Environment

The steps involve creating a key, injecting into `EnvironmentValues`, and then passing it (from parent to children).

```swift
/// 1. Declare a key with the type, and a default
struct MyEnvironmentKey: EnvironmentKey {
    static var defaultValue: CGFloat { 0 }
}

/// 2. Provide extension to EnvironmentValues
extension EnvironmentValues {
    var foo: CGFloat {
        get { self[MyEnvironmentKey.self] }
        set { self[MyEnvironmentKey.self] = newValue }
    }
}

/// 3. Pass to children using `environment`
ParentView()
  .environment(\.foo, 1.23)

/// 4. Children declare and use
@Environment(\.foo) var foo
```

NOTE: If no one sets the environment, the `defaultValue` will be used. Therefore, _some value_ will always be present. This is different from `EnvironmentObject`.

## 2. Custom EnvironmentObject

The difference is that `EnvironmentObject` does NOT require a key.

Because it simply uses the type as the "key".

You start by creating your custom type, which has to implement `ObservableObject`. Yup, that's the same view model you'll have.

```swift
/// 1. Declare the custom type
class MyModel: ObservableObject {
    @Published var foo: String = "a"
}

struct ChildView: View {

    /// 2. Use `@EnvironmentObject` with the custom type
    @EnvironmentObject var model: MyModel

    var body: some View {
        Text(model.foo)
    }
}

/// 3. Parent to pass down the model
struct ParentView: View {
    @ObservedObject var model = MyModel()

    var body: some View {
        model.foo = "b" // eg. Parent could update it
        return VStack {
            ChildView()
            ChildView()
        }
        .environmentObject(model)
    }
}
```

NOTE: You must create your `EnvironmentObject` and set it from somewhere. If it is never set and the child view uses it, the app will crash.

## PITFALL: Using in modal sheet

Environment is passed down from a parent to all it's descendant.

Typically you pass on the root view.

However, if you use a sheet, it will be a [sibling to the root](https://github.com/peterfriese/Swift-EnvironmentObject-Demo) (NOT a descendant). So you have to set the environment in the sheet.

```swift
.sheet(isPresented: $presentDetailsView) {
    DetailsView()
        .environmentObject(self.state)
}
```

## PITFALL: Xcode preview crashed

> Cannot preview in this file - YourApp.app may have crashed

This is because you didn't pass an environment for the preview. Simply pass it in your `PreviewProvider`.
