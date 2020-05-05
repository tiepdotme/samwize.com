---
layout: post
title: "Algorithm, and Moving Elements in an Array"
image: /images/meet-crusty.jpg
date: 2020-05-05T22:55:49+08:00
categories: [Algorithm, SwiftUI]
---

[WWDC 2018 session 223](https://developer.apple.com/videos/play/wwdc2018/223) is a very good session on writing good code.

It is a very casual session, with role playing. _Meet Crusty._

## The session sarcastically said of our role..

![Role of an app developer](/images/app-developer-builds-ui.jpg)

I agree. A modern day app developer spend most of the time architecting the views and models, not in writing efficient algorithm.

## Yet, algorithm matters

When a piece of code needs to scale, the **computation complexity matters**.

But there's an even more important aspect with good algorithm, and that is **clean code**.

A good algorithm is also one that reads beautifully, like a poem.

> If you want to improve the code quality in  your organization, replace all of your coding guidelines with one goal: **No Raw Loops** -- Sean Parent, C++ Seasoning

## Example: A raw loop

Let's start with a simple example where we want to remove multiple elements in an array. I have done the same before:

```swift
for i in (0..<shapes.count).reversed() {
  if shapes[i].isSelected {
    shapes.remove(at: i)
  }
}
```

The trick of using `reversed()` is needed because `remove` will mutate instantly, therefore changing the index.

The sinister thing is that `remove` is O(n). And with the **for-i raw loop**, the algorithm is O(n^2)!

A nicer and more efficient code is:

```swift
shapes.removeAll { $0.isSelected }
```

Short, sweet, and O(n)

## Example: Moving multiple elements

The session went on mentioning a few other common algorithm to do with moving multiple elements in an array.

I remember writing a hideous code trying to [re-order multiple rows of a `NSTableView`, with drag and drop](/2018/11/27/drag-and-drop-to-reorder-nstableview/)..

**In iOS 13**, you will be glad to know they have provided [`move`](https://developer.apple.com/documentation/swift/mutablecollection/3348325-move):

```swift
mutating func move(fromOffsets source: IndexSet, toOffset destination: Int)
```

It now becomes 1 line of code!

It is part of SwiftUI framework, complementary to [List](https://samwize.com/2020/04/02/everything-about-list-in-swiftui/), so you can use easily:

```swift
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
```

You might want to understand more func provided by [`MutableCollection`](https://developer.apple.com/documentation/swift/mutablecollection) and [`RangeReplaceableCollection`](https://developer.apple.com/documentation/swift/rangereplaceablecollection)

More built-in algorithms FTW 🤓
