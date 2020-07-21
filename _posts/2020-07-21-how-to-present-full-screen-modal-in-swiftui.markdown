---
layout: post
title: "How to Present Full Screen Modal in SwiftUI"
date: 2020-07-21T11:50:27+08:00
categories: [SwiftUI]
---

There is a new modifier in SwiftUI 2.0, announced in WWDC 2020, for iOS 14 (beta). _But the [API](https://developer.apple.com/documentation/swiftui/view/fullscreencover(ispresented:ondismiss:content:)) is actually back dated to iOS 13._

## fullScreenCover

It works similarly to [`sheet`](https://developer.apple.com/documentation/swiftui/view/sheet(ispresented:ondismiss:content:)), but full screen.

```swift
struct PresentFullScreen: View {
    @State var isPresentedFullScreen = false
    var body: some View {
        Button("Present Full Screen") {
            isPresentedFullScreen.toggle()
        }
        .fullScreenCover(isPresented: $isPresentedFullScreen) {
            Text("New Screen Presented")
            Button("Dismiss") {
                isPresentedFullScreen.toggle()
            }
        }
    }
}
```

## Passing an Item

You could also pass an optional Item binding. Then when it is non-nil, the view will be displayed.

So if you have an item such as `@State var book = Book()`, you can pass that into `fullScreenCover(item:)`, then you can use it in the presented view.

```swift
.fullScreenCover(item: $book) {
    BookView(book: $0)
}
```
