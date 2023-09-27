---
layout: post
title: "Conditional conformance for higher-kinded types in Swift"
description: " "
date: 2023-09-27
tags: [Swift, HigherKindedTypes]
comments: true
share: true
---

Higher-kinded types are a powerful feature in programming languages that allow us to work with generic types that have other generic types as their associated types. Swift, however, does not directly support higher-kinded types. But with **conditional conformance**, we can achieve similar results by extending protocols or types conditionally based on property or associated type constraints.

In this blog post, we will explore how to leverage conditional conformance in Swift to mimic higher-kinded types and unleash their potential.

## Understanding Higher-Kinded Types

A higher-kinded type is a generic type that takes one or more generic arguments. It allows us to work with generic types at a higher level of abstraction by abstracting over type constructors. In functional programming, higher-kinded types are often used to implement advanced concepts such as functors, monads, and applicatives.

In Swift, we can't directly define higher-kinded types. However, we can emulate them using a combination of protocols and generic types.

## Conditional Conformance

Conditional conformance is a feature in Swift that allows us to extend a generic type or protocol only when certain conditions are met. This feature enables us to add conformance to a protocol or extend a generic type based on associated type constraints or property constraints.

### Associated Type Constraints

Let's consider an example where we want to define a protocol `Functor` representing a higher-kinded type. We can start by defining the protocol and an extension that provides a default implementation for `map`:

```swift
protocol Functor {
    associatedtype A
    func map<B>(_ transform: (A) -> B) -> Self<B>
}

extension Functor {
    func map<B>(_ transform: (A) -> B) -> Self<B> {
        fatalError("map has not been implemented")
    }
}
```

Now, to enable `Functor` conformance for specific types, we can add conditional conformances based on associated type constraints. For example:

```swift
extension Array: Functor {
    typealias A = Element
    typealias B = Any
    func map<B>(_ transform: (Element) -> B) -> Array<B> {
        return self.map(transform)
    }
}
```

In the above example, we have extended the `Array` type to be a `Functor` only when the associated type `A` matches the `Element` type of the array.

### Property Constraints

Property constraints are another way to conditionally conform types to protocols. Let's consider a scenario where we want to define a protocol `EquatableWrapper` that adds `Equatable` conformance to a generic type.

```swift
protocol EquatableWrapper {
    associatedtype Wrapped
    var wrappedValue: Wrapped? { get }
}

extension EquatableWrapper where Wrapped: Equatable {
    static func ==(lhs: Self, rhs: Self) -> Bool {
        return lhs.wrappedValue == rhs.wrappedValue
    }
}
```

In this example, we have extended the `EquatableWrapper` protocol with a conditional conformance. The `==` operator is only implemented when the associated type `Wrapped` conforms to `Equatable`. This way, we can make any type that conforms to `EquatableWrapper` automatically conform to `Equatable` if the wrapped value is also `Equatable`.

### Limitations

Conditional conformance in Swift has some limitations. For example, it doesn't support protocol constraints as conditions. Therefore, you cannot define conditional conformance based on protocol constraints, only on associated types or property constraints.

## Conclusion

While Swift does not natively support higher-kinded types, we can leverage the power of conditional conformance to achieve similar results. With conditional conformance, we can extend generic types or protocols based on associated type constraints or property constraints, providing additional functionality only when specific conditions are met.

By emulating higher-kinded types using conditional conformance, we can implement advanced concepts such as functors, monads, and applicatives in Swift. This allows us to write more expressive and reusable code while leveraging the flexibility of higher-level abstractions.

#Swift #HigherKindedTypes #ConditionalConformance