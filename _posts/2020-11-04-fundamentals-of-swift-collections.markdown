---
layout: post
title: "Fundamentals of Swift Collections"
date: 2020-11-04T18:10:05+08:00
categories: [Foundation]
published: false
---

## Loop array

```swift
for value in array
for index in array.indices
for (index, value) in array.enumerated()
```

## Loop array with range

```swift
for i in 0..<array.count // Traditional loop with indices

for v in array[2...] // Skip the first 2 elements
for v in array[..<(array.count-2)] // Skip the last 2 elements
for v in array.prefix(3) // The first 3
for v in array.suffix(3) // The last 3
```

## Loop dictionary

```swift
for (key, value) in dictionary
```

## Check if dictionary has key

```swift
dictionary.keys.contains("k")
```

It is common to check with `dictionary["k"] == nil` (or use `if let`). That only works if the value type is never Optional.

## Special sequences

```swift
let tenOnes = repeatElement(1, count: 10) // Array(tenOnes)

stride(from: 0, to: 60, by: 5) // Every 5 min
```
## Array CRUD

```swift
// Find
firstIndex(of: x) // O(n)
```

```swift
// Insert
append(x) // O(1)
append(contentsOf: [..])
insert(x, at: i) // O(n)
```

It is important to note that `append` is much faster than `insert`.

```swift
// Remove
remove(at: i) // O(n)
removeAll(where: {..})
dropFirst(x)
let last = popLast() //
```

```swift
// Move
move(fromOffsets:toOffset:)
```

## Mutate

```swift
sorted()
reversed()
shuffled()
swapAt()
```

There are more using your own predicate: `max(by:)`, `sorted(by:)`, `filter()`, `map()`

## Partition

`partition(by:)` is a powerful API, and quite advanced. It reorders the sequence by a predicate, such that the right half **satisfy the predicate** (while left half does not).

It returns the index of the first index in the right half.

```swift
let p = array.partition { .. }
let h1 = list[..<p]` // Left half
let h2 = list[p...]` // Right half
```

Note that it is an unstable partition, which means the order in both half are not preserved. There is an [internal](https://github.com/apple/swift/blob/main/test/Prototypes/Algorithms.swift) `halfStablePartition` which will have the 1st half retain the original order, and also a `stablePartition` which will have both half retain the original order.

## Dictionary O(1) access

An optimizing trick is to make use of the quick O(1) for accessing a dictionary, since they are hashed.

The trade off is that the setup takes O(n), and a space of O(n).
