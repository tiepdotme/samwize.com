---
layout: post
title: "Regex Replace Using Atom"
date: 2020-08-17T10:39:34+08:00
categories: [Xcode]
---

Xcode renaming is good, but in some cases it can't help.

Often, I go back to using [Atom](https://flight-manual.atom.io/using-atom/sections/find-and-replace/) to find and replace, using regex.

## 1. Find

- Open a file (or a directory if multiple files) in Atom
- `CMD+C` to copy the string to find
- `CMD+SHIFT+F` to bring up Find panel

When you bring up Atom's Find panel, the copied string will be automatically populated in the find field, and best of all -- **automatically escape regex characters**!

![](/images/atom-find-replace.jpg)

See the `\` added in the Find textfield:

`disease\.cure\(with: "vaccine"\)`.

_NOTE: Make sure the regex mode (the `.*` option) is enabled._

## 2. Captured Group

Using the following example, we want to find _some functions with string_, and refactor them to a simple form:

```swift
// Original
disease.cure(with: "vaccine")
disease.cure(with: "mask")

// To replace and become
vaccine.asCure
mask.asCure
```

To do that, we need to add captured group for any strings (vaccine, mask, etc). We do that with `(.*?)`, capturing the shortest possible string.

We edit Find to:

`disease\.cure\(with: "(.*?)"\)`

## 3. Replace

Finally, we replace with:

`$1.asCure`

The first captured group is `$1`. If you have more captured groups, you can use `$2`, `$3` etc.

![](/images/atom-find-replace-final.jpg)
