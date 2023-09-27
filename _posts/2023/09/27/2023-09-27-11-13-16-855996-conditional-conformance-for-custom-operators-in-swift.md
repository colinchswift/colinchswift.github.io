---
layout: post
title: "Conditional conformance for custom operators in Swift"
description: " "
date: 2023-09-27
tags: [SwiftProgramming, ConditionalConformance]
comments: true
share: true
---

When working with custom operators in Swift, sometimes you may want those operators to behave differently depending on the types they are operating on. Swift allows you to achieve this through **conditional conformance**, which is a powerful feature that can make your code more flexible and expressive.

## What is Conditional Conformance?

Conditional conformance allows a generic type to conform to a protocol only under certain conditions. This means that you can define custom operators that behave differently depending on the types they are used with. For example, you might want to define an operator that concatenates two strings if they are of `String` type, but performs addition if they are of `Int` type.

## Example: Custom Operator for String Concatenation or Addition

Here's an example of how you can define a custom operator that either concatenates two strings or adds two integers depending on the types involved.

```swift
// Define the custom operator
infix operator <>: AdditionPrecedence

// Conditionally conform the generic type to the protocol
extension String: AdditiveArithmetic {
    static func <> (lhs: Self, rhs: Self) -> Self {
        return lhs + rhs
    }
}

// Usage
let string1 = "Hello"
let string2 = "World"
let result = string1 <> string2
print(result)
```

In this example, the custom operator `<>` is defined and given the `AdditionPrecedence` precedence to behave like other arithmetic operators. The `infix` keyword is used to specify that this is an infix operator.

Next, we extend the `String` type and conditionally conform it to the `AdditiveArithmetic` protocol. The `AdditiveArithmetic` protocol defines the necessary properties and methods for types that support addition and subtraction operations. By conforming `String` to this protocol, we can use the `<>` operator for concatenation.

Finally, we can use the `<>` operator to concatenate two strings (`string1` and `string2`). The result is "HelloWorld" which is printed to the console.

## Conclusion

Conditional conformance is a powerful feature in Swift that allows you to define custom operators that behave differently depending on the types they are operating on. By using this feature, you can make your code more expressive and flexible. Conditional conformance opens up a world of possibilities for creating custom operators that can work with various types in different ways.

#SwiftProgramming #ConditionalConformance