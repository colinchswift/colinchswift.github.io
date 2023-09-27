---
layout: post
title: "Conditional conformance for class types in Swift"
description: " "
date: 2023-09-27
tags: [Swift, ConditionalConformance]
comments: true
share: true
---

Class types in Swift can now conform to a protocol conditionally, making it easier to write clean and reusable code. This feature, introduced in Swift 4.1, allows you to define conformance to a protocol only when certain conditions are met.

## What is Conditional Conformance?

Conditional conformance allows a class type to conform to a protocol only under certain conditions. This means that you can specify conformances based on specific type constraints. This capability is particularly useful when dealing with generic code.

## Example Scenario

Let's say we have a protocol `EquatableToZero` that requires conforming types to provide a way to compare themselves to zero. We can define this protocol as follows:

```swift
    protocol EquatableToZero {
        static func ==(lhs: Self, rhs: Self) -> Bool
        static var zero: Self { get }
    }
```

We want to make sure that classes conforming to `EquatableToZero` can only do so if their underlying type (the type that the class is based on) also conforms to `Equatable`. Without conditional conformance, we would have to explicitly make each class conform to `Equatable` as well:

```swift
    class MyClass<T>: EquatableToZero where T: Equatable {
        static func ==(lhs: MyClass<T>, rhs: MyClass<T>) -> Bool {
            return lhs.value == rhs.value
        }
        
        static var zero: MyClass<T> {
            return MyClass(value: T.zero)
        }
    }
```

This can become tedious and cumbersome, especially if we have multiple classes that need to conform to `EquatableToZero`.

## Conditional Conformance

With conditional conformance, we can simplify our code significantly. Instead of having to manually make each class conform to `Equatable`, we can make use of the conditional conformance feature. Here's how it looks:

```swift
    class MyClass<T>: EquatableToZero where T: Equatable, T == T.Magnitude {
        static func ==(lhs: MyClass<T>, rhs: MyClass<T>) -> Bool {
            return lhs.value == rhs.value
        }
        
        static var zero: MyClass<T> {
            return MyClass(value: T.zero)
        }
    }
```

In the above code, the additional `T == T.Magnitude` clause specifies that `T` must be the same type as `T.Magnitude`. This ensures that our class only conforms to `EquatableToZero` when its underlying type also conforms to `Equatable`.

## Conclusion

Conditional conformance provides a powerful tool for writing more generic and reusable code in Swift. By allowing class types to conform to protocols only under specific conditions, you can ensure type safety and reduce code duplication. This feature is particularly valuable when dealing with generics and protocols that have additional constraints.

#Swift #ConditionalConformance