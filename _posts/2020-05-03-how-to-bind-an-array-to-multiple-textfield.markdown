---
layout: post
title: "How to Bind an Array to Multiple TextFields?"
date: 2020-05-03T15:40:33+08:00
categories: [SwiftUI]
---

## The Problem

We have a `words` state, an array of String, and we want to bind to multiple `TextField`s. To make the problem more interesting, we trigger a mutation of state only when tapping a button.

This is a naive attempt:

```swift
struct ContentView: View {
    @State var words = [String]()
    var body: some View {
        VStack {
            Button("Mutate words state") {
                self.words = ["Circuit", "Breaker"]
            }
            List {
                ForEach(words.indices) {
                    Text("\(self.words[$0])")
                }
            }
        }
    }
}
PlaygroundPage.current.liveView = UIHostingController(rootView: ContentView())
```

There are 2 difficulties in achieving a complete solution:

1. The way to loop with `ForEach`
2. Get the `Binding<String>` for the text field

## Error: `ForEach` only for constant data

The runtime error with the code above, when the button is tapped, is:

> ForEach<Range<Int>, Int, Text> count (2) != its initial count (0). `ForEach(_:content:)` should only be used for *constant* data. Instead conform data to `Identifiable` or use `ForEach(_:id:content:)` and provide an explicit `id`!

This is because there is a runtime check on the count, to ensure constant data.

The suggested solution is to have the data conform to `Identifiable`. You might try:

```swift
ForEach(words, id: \.self) { word in
    // word is String (cannot get the binding!)
}
```

But the issue with the above is that you cannot get the binding to the words.

## Solution: Use `Range<Int>`

```swift
ForEach(0..<words.count, id: \.self) { i in
    TextField("", text: self.$words[i]) // <-- and the binding
}
```

Yup, we iterate the indexes, and so we can access `$words[i]`, which is the `Binding<String>` as needed by TextField.

## A thing about binding

`$words[i]` is a binding to the array item -- a `Binding<String>` -- powered by the [binding @dynamicMemberLookup](/2019/12/04/guide-to-property-wrapper/).

We have so far used a simple example. But if the model is deep, eg `self.$viewModel.sentence.words[i]` -- the `$` (projected value) will still magically provide the binding to the property!

You can think of the `$` as being _applied_ to `viewModel.sentence.words[i]`. Or _applied_ to `viewModel.whatever.is.here`.

It will also works if the model is `@ObservedObject`, with `@Published` array of other struct items!
