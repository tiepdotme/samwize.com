---
layout: post
title: "How to Use SwiftUI and Core Data"
date: 2020-04-18T19:34:12+08:00
categories: [SwiftUI, Core Data]
---

Using SwiftUI's List is a refreshing change as we no longer use `UITableView` and `NSFetchedResultsController`.

The Core Data APIs are still the same (I covered the [CRUD](/2018/09/01/modern-guide-to-core-data-2018/) previously). The new additions are 2 helpers in the form of property wrapper.

## 1. Environment managedObjectContext

The `managedObjectContext` is passed via environment, and there is a provided system key. If you create a new project, the boilerplate will pass in in the SceneDelegate to the root view.

```swift
ContentView()
    .environment(\.managedObjectContext, context)
```

In child views, declare the environment to use.

```swift
@Environment(\.managedObjectContext) var context
```

Just like that, every children has the context, unlike some view decided to use otherwise.

## 2. @FetchRequest

This is a helper to create fetch requests in a view.

```swift
@FetchRequest(entity: Language.entity(),
              sortDescriptors: [NSSortDescriptor(...)],
              predicate: NSPredicate(...),
              animation: .spring())
var languages: FetchedResults<Language>
```

Much of it is the same as a `NSFetchRequest`, with an addition of `animation`! Indeed it is for a view.

You could have manually used `NSFetchRequest` and using the context to execute a fetch. But using `@FetchRequest` does give the view a very orderly look -- you know exactly where fetch requests are declared.

## Displaying in a list

Init `ForEach` with the variable wrapped by `@FetchRequest`. As easy as that, the list will refresh when the models are updated.

```swift
List {
    ForEach(languages) { language  in
        Text(language.name!)
    }
}
```

Oh, one thing about the Core Data model. In our example, the model `Language` needs to implement `Identifiable`.

```swift
extension Language: Identifiable {}
```

## Delete and Move

These are really powered by `List`, which I covered [here](/2020/04/02/everything-about-list-in-swiftui/). For example, delete:

```swift
ForEach(languages) { language  in
    ...
}
.onDelete {
    let language = self.languages[indexSet.first!]
    self.context.delete(language)
    try! self.context.save()
}
```

Move will be similar using `.onMove`.

## Binding optionals

Core Data models have optional properties, while SwiftUI control bindings (eg. TextField) require non-optional binding.

[AQBlog](https://alanquatermain.me/programming/swiftui/2019-11-15-CoreData-and-bindings/) provided nice extensions to binding with Core Data.

NOTE: To emphasize, it is the model's **properties** here that are being bind, NOT the model itself. You can't bind the model (read next section).

## Observing NSManagedObject

`NSManagedObject` is a class type. Since binding only works for struct, you **cannot bind `NSManagedObject`**.

But you can observe `NSManagedObject` with `@ObservedObject`, because the class is already conveniently conformed to `ObservableObject`.

```swift
// OK, and you can bind the property with eg. $model.name
@ObservedObject var model: Language

// NOT ok
@Binding var model: Language
```

When you observe the model, SwiftUI will [update when the context is saved](https://forums.swift.org/t/future-of-coredata-swiftui-structs-identifiable/30072/7).

Core Data is a framework that does not play too well with SwiftUI (yet). As [suggested](https://forums.swift.org/t/future-of-coredata-swiftui-structs-identifiable/30072/9), it would be wiser to separate the **persistent domain model** and the **view model**.
