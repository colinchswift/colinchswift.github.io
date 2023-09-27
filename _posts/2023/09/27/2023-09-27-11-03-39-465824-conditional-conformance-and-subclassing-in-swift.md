---
layout: post
title: "Conditional conformance and subclassing in Swift"
description: " "
date: 2023-09-27
tags: [Swift, ConditionalConformance]
comments: true
share: true
---

In Swift, conditional conformance is a powerful feature that allows types to conform to protocols only under certain conditions. This enables us to write more flexible and concise code. Before diving into conditional conformance, let's understand the concept of regular protocol conformance.

## Protocol Conformance in Swift

In Swift, a type can conform to a protocol by implementing all the required methods and properties defined by that protocol. For example, consider a protocol called `Equatable` that requires implementing the `==` operator:

```swift
protocol Equatable {
    static func ==(lhs: Self, rhs: Self) -> Bool
}
```

If we want to make our custom type `Person` conform to `Equatable`, we need to define the `==` operator for the `Person` type:

```swift
struct Person {
    let name: String
    let age: Int
}

extension Person: Equatable {
    static func ==(lhs: Person, rhs: Person) -> Bool {
        return lhs.name == rhs.name && lhs.age == rhs.age
    }
}

let person1 = Person(name: "John", age: 25)
let person2 = Person(name: "John", age: 25)
let areEqual = person1 == person2
```

Here, we have made the `Person` struct conform to the `Equatable` protocol by implementing the `==` operator. This allows us to compare two `Person` instances for equality using the `==` operator.

## Conditional Conformance

Conditional conformance allows types to conform to a protocol only when certain conditions are met. It is particularly useful when dealing with generic types. Let's consider an example to understand how conditional conformance works.

Suppose we have a generic type called `Container` that can hold any element conforming to the `Equatable` protocol:

```swift
struct Container<Element> {
    let element: Element
}
```

In this case, `Container` can hold any type that conforms to `Equatable`, but it doesn't need to conform to `Equatable` itself. We can achieve this using conditional conformance:

```swift
extension Container: Equatable where Element: Equatable {
    static func ==(lhs: Container<Element>, rhs: Container<Element>) -> Bool {
        return lhs.element == rhs.element
    }
}
```

Here, we have extended the `Container` struct to conditionally conform to the `Equatable` protocol only when the `Element` type also conforms to `Equatable`. The `==` operator implementation compares the elements stored in two `Container` instances.

This conditional conformance allows us to compare two `Container` instances without explicitly making the `Container` type conform to `Equatable`. For example:

```swift
let container1 = Container(element: person1)
let container2 = Container(element: person2)
let areContainersEqual = container1 == container2
```

The `==` operator invocation on `container1` and `container2` works because both `Person` and `Container` conform to `Equatable` conditions.

## Subclassing in Swift: Inheritance with Additional Functionality

In object-oriented programming, subclassing allows us to create new classes that inherit properties and functionality from existing base classes. In Swift, subclassing is an important feature that promotes code reuse and provides a way to extend functionality.

To create a subclass, we use the `class` keyword followed by the subclass name, a colon, and the base class name. Let's see an example:

```swift
class Vehicle {
    var wheels: Int
    
    init(wheels: Int) {
        self.wheels = wheels
    }
    
    func startEngine() {
        print("Engine started!")
    }
}

class Car: Vehicle {
    var color: String
    
    init(color: String) {
        self.color = color
        super.init(wheels: 4)
    }
    
    override func startEngine() {
        print("Car engine started!")
    }
}

let myCar = Car(color: "Red")
myCar.startEngine()
```

In this example, we define a `Vehicle` class with a `wheels` property and a `startEngine()` method. The `Car` class is a subclass of `Vehicle` and adds a `color` property along with its own implementation of the `startEngine()` method.

When we create an instance of `Car`, we can access both the inherited properties and methods of the base class as well as the additional properties and methods defined in the subclass. The `startEngine()` method of `Car` overrides the implementation of the same method in the base class.

Swift supports single inheritance, which means a class can inherit from only one base class. However, multiple inheritance-like behavior can be achieved using protocols and protocol composition.

By leveraging the power of conditional conformance and subclassing in Swift, we can write more flexible and reusable code that adapts to different scenarios and requirements.

#Swift #ConditionalConformance #Subclassing