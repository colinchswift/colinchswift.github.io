---
layout: post
title: "Conditional conformance for ranges in Swift"
description: " "
date: 2023-09-27
tags: [Swift, ConditionalConformance]
comments: true
share: true
---

Swift is a powerful programming language known for its expressive and concise syntax. With the release of Swift 4.2, conditional conformance was introduced, which allows types to conform to a protocol based on certain conditions. One of the interesting use cases of conditional conformance is applying it to ranges.

Ranges are widely used in Swift to represent a sequence of values. For example, you can define a range of integers like this:

```swift
let range = 1...10
```

This range includes all the integers starting from 1 up to and including 10. However, there are scenarios where you might want to operate on a range of non-integer types, such as strings or custom objects. This is where conditional conformance becomes useful.

## The problem

By default, Swift's range types only support integer values. So, if you try to create a range of non-integer types, like strings:

```swift
let range = "A"..."Z"
```

You will get a compiler error, saying that the type 'String' does not conform to the protocol 'Strideable'.

## Conditional Conformance

To make ranges work with non-integer types, we need to conditionally conform them to the 'Strideable' protocol. Conditional conformance allows you to make a type conform to a protocol only if specific conditions are met.

To extend the `Range` type to support non-integer types, we can define a custom type that handles the specific requirements of the 'Strideable' protocol for those types. Let's consider using strings for this example.

```swift
extension Range where Bound == String {
    init(_ lower: Bound, _ upper: Bound) {
        self = lower..<upper
    }
}
```

In this extension, we define a custom initializer that takes two string values as parameters. It creates a range using the `..<` operator, which represents a half-open range.

Now, we can create a range of strings like this:

```swift
let range = Range("A", "Z")
```

This code will work perfectly fine and give you a range of strings from "A" to "Z". The conditional conformance mechanism allows the compiler to infer that the `Range<String>` type can act as a range.

## Conclusion

Conditional conformance is a powerful feature in Swift that allows types to conform to a protocol under specific conditions. By applying conditional conformance to ranges, we can make them work with non-integer types such as strings or custom objects. This enables a more flexible and expressive range usage in our code.

By leveraging conditional conformance, we can unlock the full potential of ranges in Swift and write more reusable and generic code. So the next time you need to work with a range of non-integer types, remember to use conditional conformance to make it possible.

#Swift #ConditionalConformance