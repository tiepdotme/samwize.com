---
layout: post
title: "How to Handle Units (Such as Mass & Length) Using HealthKit"
date: 2015-10-26T11:20:18+08:00
categories: [iOS]
---

If you use units and quantities in your app, **HealthKit** will be very useful.

This guide will only show how to handle for **length**. But the same API works for mass, volume, time, energy etc (relevant to Health app).


## HealthKit

There are 2 classes provided by HealthKit which you can use:

1. [`HKUnit`](https://developer.apple.com/library/watchos/documentation/HealthKit/Reference/HKUnit_Class/index.html#//apple_ref/occ/clm/HKUnit/massFormatterUnitFromUnit:)
2. [`HKQuantity`](https://developer.apple.com/library/watchos/documentation/HealthKit/Reference/HKQuantity_Class/index.html#//apple_ref/occ/clm/HKQuantity/quantityWithUnit:doubleValue:)

For example, to represent _1.2345 meter_:

```swift
let meterUnit = HKUnit.meterUnit()
let m = HKQuantity(unit: meterUnit, doubleValue: 1.2345)
```

It is a 2 step process. First to create the `meterUnit` (because, length could have unit as centimeter/foot/etc). Then create the quantity with the unit and the amount as a double.


## Unit Conversions

With a `HKQuantity` object, you can get the amount:

```swift
// Return 1.2345 (m)
m.doubleValueForUnit(meterUnit)

// In centimeter - create meter unit with the prefix .Centi
let cmUnit = HKUnit.meterUnitWithMetricPrefix(.Centi)
// Return 123.45 (cm)
m.doubleValueForUnit(cmUnit)

// In foot
let footUnit = HKUnit.footUnit()
// Return 4.0501968503937 (feet)
m.doubleValueForUnit(footUnit)
```

As you can see, you always have to provide the `HKUnit`, even if you created the quantity with the `meterUnit` at first. The nice thing is that unit conversions is provided by the framework.


## Formatters

To display the unit and quantity, it is best to use a formatter.

A formatter will take care of localization and stuff, which is complex for different part of the world.

For length, we will use `NSLengthFormatter`. There are [others](http://nshipster.com/nsformatter/) for mass, energy, etc. Not all formatters are provided at the moment eg. Volume formatter is missing.

This is how you configure the formatter:

```swift
let formatter = NSLengthFormatter()
formatter.forPersonHeightUse = true
formatter.unitStyle = .Medium
```

With the formatter, you can get the string from a `HKQuantity` object:

```swift
let lengthFormatterUnit: NSLengthFormatterUnit = HKUnit.lengthFormatterUnitFromUnit(meterUnit)
let value = m.doubleValueForUnit(meterUnit)
// Returns "1.2345 m"
formatter.stringFromValue(value, unit: lengthFormatterUnit)
```

Note: The first line of code is to get the [`NSLengthFormatterUnit`](https://developer.apple.com/library/watchos/documentation/Miscellaneous/Reference/NSLengthFormatter_Class/index.html#//apple_ref/c/tdef/NSLengthFormatterUnit)(another unit object from Foundation's NSLengthFormatter class) from `meterUnit` (HealthKit unit object). Don't be confused with the 2 unit classes.


## Number Formatter

We have learnt how to print _1.2345 m_.

But how do you print to eg. 2 decimal places, or 3 significant figures?

You can can do so by configuring the [`NSNumberFormatter`](https://developer.apple.com/library/watchos/documentation/Cocoa/Reference/Foundation/Classes/NSNumberFormatter_Class/index.html#//apple_ref/occ/instp/NSNumberFormatter/roundingIncrement) which is a property of the `NSLengthFormatter`.

```swift
formatter.numberFormatter.maximumSignificantDigits = 3
formatter.numberFormatter.maximumFractionDigits = 3     // decimal places

// Returns "1.23 m"
formatter.stringFromValue(value, unit: lengthFormatterUnit)
```
