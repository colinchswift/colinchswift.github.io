---
layout: post
title: "Swift inheritance and polymorphism"
description: " "
date: 2023-10-11
tags: [inheritance, polymorphism]
comments: true
share: true
---

Inheritance and polymorphism are two important concepts in object-oriented programming that allow for code reusability and extensibility. Swift, being a powerful and expressive programming language, provides robust support for both inheritance and polymorphism. In this blog post, we will explore these concepts in detail and understand how to implement them in Swift.

## Table of Contents
- [Inheritance in Swift](#inheritance-in-swift)
- [Polymorphism in Swift](#polymorphism-in-swift)
- [Summary](#summary)

## Inheritance in Swift

Inheritance is a mechanism in which one class inherits the properties, methods, and behaviors of another class. It allows for creating a hierarchy of related classes, where subclasses inherit the characteristics of their superclass while adding their own unique features.

To define a subclass in Swift, we use the `class` keyword followed by the subclass name, a colon, and the name of the superclass. Here's an example:

```swift
class Vehicle {
    var speed: Double = 0.0
    
    func accelerate() {
        // code to accelerate the vehicle
    }
}

class Car: Vehicle {
    var numberOfSeats: Int = 5
}
```

In the above example, the `Car` class inherits from the `Vehicle` class. The `Car` class automatically gains access to the `speed` property and the `accelerate()` method defined in the `Vehicle` class. Additionally, the `Car` class introduces its own property called `numberOfSeats`.

## Polymorphism in Swift

Polymorphism is the ability of an object to take on many different forms. In Swift, polymorphism is achieved through method overriding, where a subclass provides its own implementation of a method that is already defined in its superclass.

To override a method in Swift, we use the `override` keyword before the method declaration. Here's an example:

```swift
class Shape {
    func draw() {
        // code to draw the shape
    }
}

class Circle: Shape {
    override func draw() {
        // code to draw a circle
    }
}

class Rectangle: Shape {
    override func draw() {
        // code to draw a rectangle
    }
}
```

In the above example, the `Circle` and `Rectangle` classes override the `draw()` method inherited from the `Shape` class. This allows us to treat objects of different classes as instances of a common superclass, enabling polymorphism.

## Summary

Inheritance and polymorphism are powerful techniques in object-oriented programming that enhance code reusability and flexibility. Swift provides excellent support for these concepts, allowing for the creation of hierarchies of related classes and the ability to take on multiple forms through method overriding.

By understanding and utilizing inheritance and polymorphism effectively in Swift, you can create more robust and extensible code structures that are easier to maintain and enhance.

**#swift #inheritance #polymorphism**