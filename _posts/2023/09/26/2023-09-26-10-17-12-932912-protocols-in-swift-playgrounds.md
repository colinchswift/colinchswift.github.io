---
layout: post
title: "Protocols in Swift Playgrounds"
description: " "
date: 2023-09-26
tags: [Swift, Protocols]
comments: true
share: true
---

In Swift, protocols play a crucial role in making your code more flexible, reusable, and interoperable. They allow you to define a set of methods, properties, or other requirements that a type must conform to. This enables you to write generic code that can work with any type that adheres to the protocol, helping to achieve better code organization and reducing redundancy.

## Defining a Protocol

To define a protocol in Swift, you use the `protocol` keyword followed by the name of the protocol. Inside the protocol, you can declare methods, properties, and other requirements like optional methods or properties.

Let's take a simple example of a `Drawable` protocol that defines a requirement for an object to be able to draw on the screen. We can define this protocol as follows:

```swift
protocol Drawable {
    func draw()
}
```

In this example, we have defined a single method requirement `draw()` that any type adopting the `Drawable` protocol must implement.

## Protocol Conformance

To make a type conform to a protocol, you simply need to implement all the requirements declared by that protocol. Let's say we have a `Circle` class and a `Rectangle` class, both of which can be drawn. We can make them conform to the `Drawable` protocol like this:

```swift
class Circle: Drawable {
    func draw() {
        // Code to draw a circle
    }
}

class Rectangle: Drawable {
    func draw() {
        // Code to draw a rectangle
    }
}
```

Here, both the `Circle` and `Rectangle` classes implement the `draw()` method, fulfilling the requirement of the `Drawable` protocol.

## Protocol Inheritance

Similar to classes and structs, protocols can also inherit from other protocols. This allows you to build a hierarchy of protocols, each refining the requirements of the previous one. For example, let's consider a scenario where we have a `Shape` protocol and we want to make the `Drawable` protocol inherit from it:

```swift
protocol Shape {
    func calculateArea() -> Double
}

protocol Drawable: Shape {
    func draw()
}
```

In this case, the `Drawable` protocol inherits from the `Shape` protocol, adding the requirement of the `draw()` method on top of the existing requirement of `calculateArea()`. Now, any type conforming to the `Drawable` protocol needs to implement both methods.

## Using Protocols to Provide Default Implementations

Swift allows you to provide default implementations for methods and properties in protocols. This is particularly useful when you have a set of common functionality that can be shared among conforming types. Let's modify our `Shape` protocol to include a default implementation for the `calculateArea()` method:

```swift
protocol Shape {
    func calculateArea() -> Double
}

extension Shape {
    func calculateArea() -> Double {
        // Default implementation
        return 0.0
    }
}
```

Now, any type that conforms to the `Shape` protocol will inherit this default implementation of the `calculateArea()` method. However, conforming types can still choose to override this default implementation with their own custom implementation if needed.

## Conclusion

Protocols in Swift provide a powerful mechanism to define reusable blueprints for types, enabling you to write more flexible and interoperable code. By adhering to protocols, your types become part of a common interface, allowing them to be used interchangeably and promoting code organization. With default implementations, you can further enhance the flexibility of your protocols. So, next time you find yourself needing to define a set of requirements, consider using protocols in Swift to make your code more modular and maintainable.

#Swift #Protocols