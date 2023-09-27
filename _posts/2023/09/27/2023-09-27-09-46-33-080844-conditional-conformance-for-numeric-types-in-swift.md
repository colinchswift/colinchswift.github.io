---
layout: post
title: "Conditional conformance for numeric types in Swift"
description: " "
date: 2023-09-27
tags: [programming]
comments: true
share: true
---

Swift is a powerful programming language that offers extensive support for working with various numeric types like integers, floating-point numbers, and more. With the introduction of Swift 5.3, a new feature called conditional conformance was added, which allows numeric types to have additional conformance to certain protocols based on their underlying capabilities.

## What is conditional conformance?

Conditional conformance in Swift allows a generic type to conform to a protocol only when certain conditions are met. This means that the conformance is determined dynamically based on the capabilities or properties of the concrete type being used.

In the context of numeric types, conditional conformance allows a generic type that represents a numeric value to automatically conform to certain protocols, such as `Comparable`, `Equatable`, and `Hashable`, depending on its underlying properties.

## Example: Conforming to the Comparable protocol

Let's take an example of a generic `Number` struct that represents a numeric value:

```swift
struct Number<Value: Numeric> {
    let value: Value
}
```

By default, the `Number` struct doesn't automatically conform to the `Comparable` protocol, as the `Numeric` protocol doesn't imply any comparison operations. However, we can use conditional conformance to make `Number` conform to `Comparable` when the underlying value type also conforms to `Comparable`. Here's how we can achieve that:

```swift
extension Number: Comparable where Value: Comparable {
    static func < (lhs: Number<Value>, rhs: Number<Value>) -> Bool {
        return lhs.value < rhs.value
    }
    
    static func == (lhs: Number<Value>, rhs: Number<Value>) -> Bool {
        return lhs.value == rhs.value
    }
}
```

In the above example, we conditionally extend the `Number` struct to conform to the `Comparable` protocol only when the underlying `Value` type conforms to `Comparable`. We then implement the less than (`<`) and equal to (`==`) operators based on the comparison capability of the underlying value type.

## Example usage

Once we have defined the conditional conformance, we can use the `Number` struct with any numeric type that also conforms to `Comparable`. Here are a few examples:

```swift
let intNumber1 = Number(value: 10)
let intNumber2 = Number(value: 5)

print(intNumber1 < intNumber2)  // Output: false
print(intNumber1 > intNumber2)  // Output: true
print(intNumber1 == intNumber2) // Output: false

let doubleNumber1 = Number(value: 3.14)
let doubleNumber2 = Number(value: 2.71)

print(doubleNumber1 < doubleNumber2)  // Output: false
print(doubleNumber1 > doubleNumber2)  // Output: true
print(doubleNumber1 == doubleNumber2) // Output: false
```

In the above usage examples, the `Number` struct automatically conforms to the `Comparable` protocol because `Int` and `Double` types also conform to `Comparable`.

## Conclusion

Conditional conformance for numeric types in Swift provides a more flexible and powerful way to work with generic numeric values. It allows generic types to automatically conform to protocols like `Comparable`, `Equatable`, and `Hashable` based on the capabilities of their underlying numeric types. This feature enhances the expressiveness and readability of Swift code when dealing with generic numeric operations.

#swift #programming