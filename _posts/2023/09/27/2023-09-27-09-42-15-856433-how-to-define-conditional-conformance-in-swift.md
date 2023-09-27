---
layout: post
title: "How to define conditional conformance in Swift"
description: " "
date: 2023-09-27
tags: [Swift, ConditionalConformance]
comments: true
share: true
---

In Swift, conditional conformance allows you to define conformance to a protocol only under certain conditions. This allows you to write more flexible and specialized code, as you can define conformance based on specific requirements or constraints.

To define conditional conformance in Swift, you can use the `where` clause in the protocol extension. The `where` clause allows you to specify additional conditions on the associated types or generic types.

Here's an example that demonstrates how to define conditional conformance in Swift:

```swift
protocol Numeric {
    static func +(lhs: Self, rhs: Self) -> Self
    static func *(lhs: Self, rhs: Self) -> Self
}

extension Int: Numeric {}
extension Double: Numeric {}

struct Vector2D<T: Numeric> {
    var x: T
    var y: T
}

extension Vector2D: Equatable where T: Equatable {}
extension Vector2D: Hashable where T: Hashable {}
```

In this example, we define a protocol called `Numeric` that defines basic arithmetic operations. We then extend `Int` and `Double` to conform to this protocol.

Next, we define a `Vector2D` struct that takes a generic type `T`, which conforms to the `Numeric` protocol. This struct represents a 2D vector.

Finally, we use conditional conformance to extend the `Vector2D` struct and specify that it conforms to the `Equatable` and `Hashable` protocols only when the generic type `T` also conforms to those protocols.

By using conditional conformance, we can ensure that the vector's equality and hashability are based on the equality and hashability of its underlying numeric type.

Conditional conformance is a powerful feature in Swift that allows you to make your code more expressive and adaptable without sacrificing type safety. It's particularly useful when working with generic code or when dealing with different types that require different conformances.

#Swift #ConditionalConformance