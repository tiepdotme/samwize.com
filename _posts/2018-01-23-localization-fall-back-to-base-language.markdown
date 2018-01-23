---
layout: post
title: "Localization - Fallback to Base Language"
date: 2018-01-23T15:49:41+08:00
categories: [Localization, iOS]
---

This post explains how iOS determines the language to use in an app, and how Apple fallback to the next-best default language when necessary.

Throughout this post, we use the example where `en` (English) is the developmen and base language, while `zh-Hans` (Chinese) is an additional supported language.

```
# en Localizable.strings
"awesome-title" = "Hello World";
"another-title" = "Localization Rocks";
```

```
# zh-Hans Localizable.strings
"awesome-title" = "你好";
```

Deliberately, the `zh-Hans` strings file is 50% localized (`another-title` is not translated).

## How Apple determines the langauge

Apple [explains the process](https://developer.apple.com/library/content/qa/qa1828/_index.html). Here is the "alogrithm" in pseudocode:

```
func determineTheLanguageToUse():
  for each user's preferredLanguages
    if app supports the language
      return the language
    if app supports a more generic dialect
      return the generic language

  # Exhausted preferredLanguages and still cannot determine..
  return CFBundleDevelopmentRegion
```

User's `preferredLanguages` is those listed in **Settings App > General > Language & Region**.

The algo starts with the most preferred language, checks if the app supports it (or a more generic dialect), before finally using `CFBundleDevelopmentRegion`.

There are different cases of fallback. Let's look at them in detail.

## Fallback 1: Generic Dialect

In the algo, iOS will check if there is a more generic dialect for the preferred language, and if so return that.

What is a **more generic dialect**? `en` is more generic than `en-GB` (British English). In our example, if user prefers `en-GB`, then `en` will be used.

This is because the app does NOT have `en-GB`, but `en` is good enough.

The other way round is not true. If the app supports `en-GB` (and not `en`), then if user prefers `en`, then `en-GB` will not be the fallback -- because `en-GB` is _not_ more generic.

## Fallback 2: Unsupported Language

An unsupported language is when all `preferredLanguages` is exhausted, and the app does not have a suitable language to use.

For example if a user prefers `ms` (Malay), but which the app does not support at all, then the language specified in `CFBundleDevelopmentRegion` of the Info.plist will be used. This is aka the **localization native development region**.

This is a very common case. When we lanuch an app, we probably support only a few languages, or just one!

## Fallback 3: Unsupported Phrase

This is an obscure case, and is not mention in Apple's documentation, nor in the alogrithm.

Let's take the example where a user prefers `zh-Hans`.

What happens when iOS try to get the `NSLocalizedString` of "another-title"?

Remember, `zh-Hans` is not fully translated. It does not have "another-title". What do you think will happen?

1. Fallback to development language `en` or
2. Return "another-title"

Many developers think it is (1). Unfortunately, it is (2), with that ugly key name!

If you refer to the `determineTheLanguageToUse` algo, the language to use is still `zh-Hans`, regardless that it is incomplete. It can't find "another-title", so it just return the key as value..

iOS should really improve on this fallback behaviour.. for now, we need some custom code.

## The Fallback Code

`LS` is a global function to replace `NSLocalizedString`. It is a shorthand, with added fallback capability:

```swift
public func LS(_ key: String) -> String {
    let value = NSLocalizedString(key, comment: "")
    if value != key || NSLocale.preferredLanguages.first == "en" {
        return value
    }

    // Fall back to en
    guard
        let path = Bundle.main.path(forResource: "en", ofType: "lproj"),
        let bundle = Bundle(path: path)
        else { return value }
    return NSLocalizedString(key, bundle: bundle, comment: "")
}
```

When a phrase is not yet translated, it will be `value == key`, which is dumb, so we fall back to using `en`.

The rest of the code is simply geting `NSLocalizedString` from the en bundle.
