---
layout: post
title: "42 Most Common SwiftUI API That I Can't Remember"
date: 2020-06-08T13:34:41+08:00
categories: [SwiftUI]
---

This is a list of SwiftUI code snippets that I will be updating continuously. They are not hard, just not easy to remember when you have been using UIKit for 10 years. Some are more like workarounds :) **Not yet 42**, but I believe I can reach the magic number as I continue to use SwiftUI.

## Resizable image, fits in frame

```swift
Image(systemName: "umbrella")
    .resizable()
    .aspectRatio(contentMode: .fit)
    .frame(width: 50, height: 50)
```

## Increase hit area, including transparent area

```swift
HStack {
    Text("When should you test yourself?")
    Spacer() // Without `contentShape`, this space is not hittable
}
.contentShape(Rectangle()) // Define hit testing
.onTapGesture { ... }
```

## Custom Modifier

```swift
struct MyModifer: ViewModifier {
    func body(content: Content) -> some View {
        // Do something with the content
    }
}

// Convenient method on View
extension View {
    func myModifer() -> some View {
        self.modifier(MyModifer())
    }
}
```

## Dismiss modal view

```swift
@Environment(\.presentationMode) var presentationMode

presentationMode.wrappedValue.dismiss()
```

Actually, `dismiss()` is not for just modal view. If the view is in a navigation, it will "Go Back" -- which is not the _dismiss_ we expect. There's another way.

## Dismiss modal view 2 -- the explicit way

In the presenter,

```swift
@State private var isPresented = false // Declare an @State

isPresented = true // when it's time to present

.sheet(isPresented: $isPresented) { // The modal sheet uses the binding
    DeeperView(isPresented: self.$isPresented) // Also pass the binding to any children
}
```

In the child views of the navigation stack,

```swift
@Binding var isPresented: Bool // Declare the binding

isPresented = false // when it's time to dismiss the modal for real
```

## FIX Text truncating when it shouldn't

This usually happens when the `Text` is in `VStack` contained in a `ScollView`.

```swift
Text("Why the hell is this text truncated without reason?")
.fixedSize(horizontal: false, vertical: true) // Workaround magic
```

## Select an item using Picker in a Form

Picker can show the currently selected item, but somehow only works of the selection is Int/String.

```swift
// Assuming we have an enum to represent the list of items
enum Side: CaseIterable {
    case wife, husband
}

@State var sideIndex = 0

Picker("Whose side you taking?", selection: $sideIndex) {
    ForEach(Side.allCases.indices) {
        Text(Side.allCases[$0].description)
    }
}
```
