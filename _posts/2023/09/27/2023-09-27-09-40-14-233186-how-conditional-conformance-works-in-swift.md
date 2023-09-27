---
layout: post
title: "How conditional conformance works in Swift"
description: " "
date: 2023-09-27
tags: [Swift, ConditionalConformance]
comments: true
share: true
---

Conditional conformance is a powerful feature in Swift that allows you to apply a protocol to a generic type only when certain conditions are met. This feature unlocks additional flexibility and expressiveness in your code. In this article, we will explore how conditional conformance works in Swift and how you can leverage it to write more efficient and reusable code.

## Basics of Conformance in Swift

In Swift, a type can conform to a protocol by implementing all the required methods and properties defined in that protocol. This conformance is usually determined by the capabilities and characteristics of the type itself. For example, a `Person` struct can conform to a `Comparable` protocol if it provides an implementation for the `<` operator.

## Introduction to Conditional Conformance

Conditional conformance extends the concept of protocol conformance by allowing you to specify additional constraints on the generic type. This means that a generic type can conform to a protocol only when certain conditions are satisfied.

For example, consider a generic `Optional` type that can represent either a value or `nil`. By default, Swift's `Optional` type does not conform to the `Equatable` protocol because `nil` cannot be compared to any other value. However, with conditional conformance, you can write an extension to `Optional` that specifies it conforms to `Equatable` when the underlying type is also `Equatable`.

```swift
extension Optional: Equatable where Wrapped: Equatable {
    static func ==(lhs: Optional<Wrapped>, rhs: Optional<Wrapped>) -> Bool {
        switch (lhs, rhs) {
        case (.none, .none):
            return true
        case let (.some(leftValue), .some(rightValue)):
            return leftValue == rightValue
        default:
            return false
        }
    }
}
```

In this example, the conformance to `Equatable` is conditionally applied only when the `Wrapped` type is itself `Equatable`. This allows us to compare two `Optional` values, as long as the underlying types are also equatable.

## Benefits of Conditional Conformance

Conditional conformance in Swift provides several benefits. Firstly, it allows you to write more generic code that can adapt to different constraints. This means that you can define protocols that only require certain conditions to be met by the conforming type.

Secondly, conditional conformance enhances code reusability. By allowing types to conform to protocols conditionally, you can avoid code duplication and improve the overall architecture of your application.

Lastly, conditional conformance enables more efficient code generation. Swift's compiler performs type checking and generates specialized code when the conditions for conformance are met. This results in optimized code execution and potentially smaller binary sizes.

## Conclusion

Conditional conformance is a powerful feature in Swift that adds flexibility and expressive capabilities to your code. By applying protocols to generic types conditionally, you can write more efficient and reusable code. Understanding how conditional conformance works will enable you to leverage this feature effectively in your Swift projects.

#Swift #ConditionalConformance