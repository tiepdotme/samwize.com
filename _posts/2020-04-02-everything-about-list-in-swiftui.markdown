---
layout: post
title: "Everything About List in SwiftUI"
date: 2020-04-02T20:57:27+08:00
categories: [SwiftUI]
---

`List` is the equivalent of `UITableView` or `NSTableView`.

## A simple list of items

```swift
struct ListDemo: View {
    var items = ["China", "United States"]

    var body: some View {
        List(items, id: \.self) { item in
            Text(item)
        }
        .listStyle(GroupedListStyle())
    }
}
```

List requires an item's keypath, to use it as an identifier. For this array of String, simply use itself -- `\.self`. Custom models should implement the protocol `Identifiable`.

The example above uses `GroupedListStyle()`.

## Selecting a row

There is no `UITableViewDelegate` to callback when a row is selected.

The following is a workaround.

```swift
List(items, id: \.self) { item in
    HStack {
        Text(item)
        Spacer()
    }
    .contentShape(Rectangle())
    .onTapGesture {
        // Handle the item
    }
```

The `HStack` is used as a row, with a spacer so that the whole width is filled.

The `contentShape(Rectangle())` makes it possible to tap on the `Spacer()`!

> `contentShape` defines the content shape for hit testing.

## Dismiss the view

This is not about a list, but after selecting a row, you can dismiss the modal view using `presentationMode`.

```swift
// Declare the environment
@Environment(\.presentationMode) var presentationMode

// Call dismiss
.onTapGesture {
    self.presentationMode.wrappedValue.dismiss()
}
```

## Change row background

The row background can be set with `listRowBackground()`. But our code above will _somehow_ not work. eg. `HStack(...).listRowBackground(Color.blue)` will do nothing.

Strangely, but using `ForEach` will work.

```swift
List {
    ForEach(items, id: \.self) { item in
        Text(item)
            .listRowBackground(Color.blue)
    }
}
```

And that brings us to the _next section_, which tells us `ForEach` should be preferred over convenience `List.init(_: id:)`.

## Section, and the use of `ForEach`

You need multiple `ForEach` when you have multiple sections in the table.

```swift
List() {
    Section(header: Text("Section 1"), footer: Color.blue) {
        ForEach(items, id: \.self) { item in
            Text(item)
        }
    }

    // Another section
    Section() { }

    // In fact you can mix any kind of view in a List
    Image(systemName: "bolt")
}
```

## Edit, delete, and move

You handle with `onDelete()` and `onMove()` in the `ForEach` (NOT `List`).

```swift
struct EditListDemo: View {
    @State var items = ["iPhone", "iPad", "Apple Watch", "Apple TV"]

    var body: some View {
        List {
            ForEach(items, id: \.self) { item in
                Text(item)
            }
            .onDelete { set in
                items.remove(atOffsets: set)
            }
            .onMove { set, i in
                items.move(fromOffsets: set, toOffset: i)
            }
        }
        .navigationBarItems(trailing: EditButton())
    }
}
```

## Drag and drop

Drag and drop is via the `onInsert()` method.

```swift
// Must import this for the UTI types
import MobileCoreServices

.onInsert(of: [String(kUTTypeText)]) { i, itemProviders in
    for provider in itemProviders {
        if provider.canLoadObject(ofClass: String.self) {
            _ = provider.loadObject(ofClass: String.self) { s, error in
                self.items.insert(s!, at: i)
            }
        }
    }
}
```

The above handles 1 type -- a text. It can be multiple types.

The `NSItemProvider`s will be able to resolve the type, and you insert into your items.
