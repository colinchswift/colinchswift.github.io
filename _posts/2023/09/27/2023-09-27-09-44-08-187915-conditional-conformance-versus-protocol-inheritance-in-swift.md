---
layout: post
title: "Conditional conformance versus protocol inheritance in Swift"
description: " "
date: 2023-09-27
tags: [Protocols]
comments: true
share: true
---

When working with protocols in Swift, there are two fundamental approaches to extend their functionality and reuse code: **protocol inheritance** and **conditional conformance**. These two concepts provide different mechanisms for extending protocols and have their own advantages and use cases. In this blog post, we'll explore the differences between them and see when to use each approach.

## Protocol Inheritance

Protocol inheritance allows you to define a new protocol by inheriting from an existing one. This means that the new protocol inherits all the requirements of the base protocol and can add additional ones as needed. By conforming to the new protocol, a type is also implicitly conforming to the base protocol.

Here's an example to illustrate protocol inheritance:

```swift
protocol Vehicle {
    func startEngine()
    func stopEngine()
}

protocol Car: Vehicle {
    func drive()
}

struct Sedan: Car {
    func startEngine() {
        // Start the sedan's engine
    }
    
    func stopEngine() {
        // Stop the sedan's engine
    }
    
    func drive() {
        // Drive the sedan
    }
}
```

In this example, the `Car` protocol inherits from the `Vehicle` protocol, adding the `drive()` requirement. The `Sedan` struct conforms to the `Car` protocol, which means it has to implement all the requirements from both protocols.

Protocol inheritance is useful when you want to define a specialized version of an existing protocol or when you want to create a hierarchy of protocols with increasing levels of abstraction.

## Conditional Conformance

Conditional conformance, introduced in Swift 4.1, allows a type to conditionally conform to a protocol based on specific constraints. This is particularly helpful when you want to extend a generic type to conform to a protocol only if certain conditions are met.

Here's an example to illustrate conditional conformance:

```swift
protocol Printable {
    func print()
}

extension Array: Printable where Element: Printable {
    func print() {
        for element in self {
            element.print()
        }
    }
}

struct Person {
    let name: String
}

extension Person: Printable {
    func print() {
        print("Person name: \(name)")
    }
}
```

In this example, the `Printable` protocol defines a single requirement: the `print()` method. The `Array` type is extended to conform to `Printable` only if its `Element` type also conforms to `Printable`. The `Person` struct also conforms to `Printable` and provides its own implementation for the `print()` method.

Conditional conformance is useful when you want to extend generic types to conform to protocols based on specific type constraints or when you want to provide a default implementation for a protocol based on certain conditions.

## Conclusion

Protocol inheritance and conditional conformance are powerful features in Swift for extending protocol functionality and enabling code reuse. Protocol inheritance is useful for defining specialized versions of existing protocols or creating protocol hierarchies. On the other hand, conditional conformance allows generics to conform to protocols based on specific constraints.

Understanding when to use each approach is crucial for writing clean and maintainable code. By choosing the appropriate technique, you can effectively extend your protocols and make your code more flexible and reusable.

#Swift #Protocols