---
layout: post
title: "A Guide to NSNumberFormatter"
date: 2015-11-04T09:23:06+08:00
categories: [iOS]
---

[NSNumberFormatter](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSNumberFormatter_Class/) is a helpful class when you need to display numbers as well-formatted string.

However, it is pretty tricky (in fact misleading) when you are configuring the number of significant figures (SF) or decimal places (DP), hence this guide.

## Defaults

When you init a `NSNumberFormatter`, the default is:

- `usesSignificantDigits` = `false` 
- `minimum/maximumSignificantDigits` = 1-6
- `minimum/maximumFractionDigits` = 0-0

`usesSignificantDigits` = `false` means the formatter will not use the significant digits (aka SF), even though it is set to 1-6. And since the fractional digits (aka DP) is set to 0-0, it will not show any decimal places.

```swift
let num = 1.23456789
var numberFormatter = NSNumberFormatter()
print("SF: \(numberFormatter.minimumSignificantDigits)-\(numberFormatter.maximumSignificantDigits) [Uses SD: \(numberFormatter.usesSignificantDigits)]")
print("DP: \(numberFormatter.minimumFractionDigits)-\(numberFormatter.maximumFractionDigits)")

/// Prints "1"
numberFormatter.stringFromNumber(num)
```


## Set the Decimal Places

This is straight forward:

```swift
numberFormatter.maximumFractionDigits = 3
/// Prints "1.235" (3 DP)
numberFormatter.stringFromNumber(num)
```

Also note that it actually rounded up, because the default mode is `RoundHalfEven`.


## Set the Significant Figures

It gets trickier.

If you want to use significant figures, you can set `usesSignificantDigits` to `true` to activate the default.

```swift
numberFormatter.usesSignificantDigits = true
/// Prints "1.23457" (6 SF)
numberFormatter.stringFromNumber(num)
```

But you can also just set `minimum/maximumSignificantDigits`, with a caveat.. 

```swift
numberFormatter = NSNumberFormatter()
print(numberFormatter.usesSignificantDigits)    // "false" by default
numberFormatter.minimumSignificantDigits = 1    // Set the SF
print(numberFormatter.usesSignificantDigits)    // "true", somehow it auto set
numberFormatter.usesSignificantDigits = false   // Reset to false
print(numberFormatter.usesSignificantDigits)    // "false"
numberFormatter.minimumSignificantDigits = 1    // Subsequent set
print(numberFormatter.usesSignificantDigits)    // Remains false
```

In line 3, setting `minimumSignificantDigits` causes `usesSignificantDigits` to be auto set to `true`.

But when you reset `usesSignificantDigits` to `false` (line 6) and then subequently set `minimum/maximumSignificantDigits` (line 7), it will NOT auto set `usesSignificantDigits` to `true`!

The behaviour in this API is badly designed.

To prevent this misleading behaviour, remember to set `usesSignificantDigits` explicitly everytime.


## Either Decimal Places or Significant Figures

On any cases, the formatting will use either `minimum/maximumSignificantDigits` or `minimum/maximumFractionDigits`.

It will NOT use both configurations.

So control with `usesSignificantDigits`.

An example of a function that control the use of DP or SF depending on the number:

```swift
numberFormatter.maximumSignificantDigits    = 4
numberFormatter.maximumFractionDigits       = 2

// If num > 1, use DP, else use SF
func stringFromNum(num: Double) -> String? {
    if num > 1 {
        numberFormatter.usesSignificantDigits = false
    } else {
        numberFormatter.usesSignificantDigits = true
    }
    return numberFormatter.stringFromNumber(num)
}

stringFromNum(0.123456789)      // "0.1235" (4 SF)
stringFromNum(9876.123456789)   // "9876.12" (2 DP)
```


## Bug with usesSignificantDigits

It is not the end of the story. 

There is a [serious bug](http://www.openradar.me/18468186) with `usesSignificantDigits`, and it is still not fixed since Sep 2014.

Reproduce bug:

```swift
numberFormatter = NSNumberFormatter()
numberFormatter.maximumSignificantDigits = 2
numberFormatter.usesSignificantDigits = false
numberFormatter.stringFromNumber(3.8765432)  // Expected "4" (0 DP), but printed "3.9" (2 SF)
```

Solution:

```swift
numberFormatter = NSNumberFormatter()
numberFormatter.stringFromNumber(0)    // This is the WORKAROUND
numberFormatter.maximumSignificantDigits = 2
numberFormatter.usesSignificantDigits = false
numberFormatter.stringFromNumber(3.8765432)
```

As weird as it seems, using `stringFromNumber` once right after init-ing will set things correct!

Not sure why, but that is a workaround..
