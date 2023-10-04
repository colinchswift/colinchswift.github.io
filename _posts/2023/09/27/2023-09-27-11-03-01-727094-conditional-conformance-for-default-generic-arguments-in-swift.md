---
layout: post
title: "Conditional conformance for default generic arguments in Swift"
description: " "
date: 2023-09-27
tags: [ConditionalConformance]
comments: true
share: true
---

Swift 5.3 introduced the feature of **conditional conformance** for generic types and protocols. This powerful feature allows you to write code that conditionally conforms to a protocol based on certain requirements. One specific use case where this feature is beneficial is when working with **default generic arguments**.

## What are default generic arguments?

Default generic arguments allow you to provide a default value for a generic parameter in a function or type. This means that when you define a generic function or type, you can specify a default value for one or more of its generic parameters. If a concrete type is not explicitly provided for those parameters, the default values will be used.

```swift
func processData<T: Codable>(data: T, encoding: String.Encoding = .utf8) {
    // Process data here
}
```

In the example above, the `encoding` parameter has a default value of `.utf8`. This means that if you call this function without providing an explicit value for `encoding`, it will default to `.utf8`.

## Conditional conformance with default generic arguments

Prior to Swift 5.3, it was not possible to conditionally conform to a protocol based on the default generic argument of a type or function. This limitation made it challenging to write code that conditionally conformed to a protocol when certain conditions were met.

With Swift 5.3, you can now take advantage of **conditional conformance** in combination with default generic arguments. This means that you can write code that conditionally conforms to a protocol based on the default value of a generic parameter.

```swift
protocol StringRepresentable {
    var stringValue: String { get }
}

extension Array: StringRepresentable where Element: CustomStringConvertible {
    var stringValue: String {
        // Convert array to string representation
        return self.map { $0.description }.joined(separator: ", ")
    }
}

let numbers = [1, 2, 3, 4, 5]
let stringRepresentation = numbers.stringValue
```

In the example above, the `Array` type conditionally conforms to the `StringRepresentable` protocol when its `Element` type conforms to `CustomStringConvertible`. This allows you to call the `stringValue` property on an array of elements that are `CustomStringConvertible`, and get a string representation of the array.

## Conclusion

Conditional conformance for default generic arguments in Swift 5.3 opens up new possibilities for writing more flexible and reusable code. It allows you to conditionally conform to protocols based on the default values of generic parameters, providing more control over the behavior of your code. With this feature, you can write cleaner and more concise code that adapts to different scenarios.

#Swift #ConditionalConformance