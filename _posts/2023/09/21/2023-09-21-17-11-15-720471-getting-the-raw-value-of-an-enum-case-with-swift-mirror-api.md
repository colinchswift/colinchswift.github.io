---
layout: post
title: "Getting the raw value of an enum case with Swift Mirror API"
description: " "
date: 2023-09-21
tags: [SwiftProgramming, SwiftEnums]
comments: true
share: true
---

Enums in Swift provide a powerful way to define a group of related values. Each case of an enum can have an associated value, making it a flexible way to represent different states or options in your code. Sometimes, you may need to extract the raw value of an enum case, especially when working with APIs or storing enum values in a database.

In Swift, the Mirror API allows you to inspect the properties of a type, including enums. By leveraging this API, you can easily access the raw value of an enum case. Let's see how it can be done.

## Setting Up the Enum

First, let's set up an example enum that represents different car brands:

```swift
enum CarBrand: String {
    case toyota
    case honda
    case ford
}
```

In this example, each case has a raw value of type `String`.

## Getting the Raw Value

To get the raw value of an enum case, we can use the `Mirror` API along with a `switch` statement. Here's an example of how to do it:

```swift
func getRawValue(of carBrand: CarBrand) -> String? {
    let mirror = Mirror(reflecting: carBrand)
    guard mirror.displayStyle == .enum else {
        return nil
    }
    
    for child in mirror.children {
        if let rawValue = child.value as? String {
            return rawValue
        }
    }
    
    return nil
}
```
Here, the `getRawValue` function takes an instance of the `CarBrand` enum and returns its raw value as a `String`. It uses the `Mirror(reflecting:)` initializer to create a mirror reflection of the enum instance. Then, it checks if the `displayStyle` of the mirror is `.enum` to ensure it is an enum.

Next, the function iterates over the `children` property of the mirror, looking for a child whose value matches the raw value type (`String`). If found, it returns the raw value; otherwise, it returns `nil`.

## Usage Example

Let's see the `getRawValue` function in action:

```swift
let car = CarBrand.toyota
if let rawValue = getRawValue(of: car) {
    print("Raw Value:", rawValue)
} else {
    print("Unable to get raw value.")
}
```

This code creates an instance of `CarBrand` enum and passes it to `getRawValue` function. If the raw value is successfully extracted, it will be printed; otherwise, it will display an error message.

## Summary

The Swift `Mirror` API provides a convenient way to access the raw value of an enum case. By leveraging the mirror reflection, you can easily extract the raw value and utilize it in various scenarios. This is especially useful when working with external APIs or when storing enum values in a database.

#SwiftProgramming #SwiftEnums