---
layout: post
title: "Inheritance in Swift Playgrounds"
description: " "
date: 2023-09-26
tags: [Swift, Inheritance]
comments: true
share: true
---

Inheritance is a fundamental concept in Swift and object-oriented programming (OOP) in general. With inheritance, you can create new classes that inherit the characteristics and behavior of an existing class, allowing you to extend and customize it to meet your specific needs. In this blog post, we'll explore how to use inheritance in Swift Playgrounds to build upon existing classes.

## Basics of Inheritance

Inheritance in Swift follows a parent-child relationship, where the child class inherits properties, methods, and other characteristics from the parent class. The parent class is also known as the superclass, while the child class is called the subclass.

To create a subclass that inherits from a superclass, you use the `class` keyword followed by the subclass name. After the subclass name, you specify the superclass by using a colon, followed by the superclass name. Here's an example:

```swift
class Vehicle {
    var brand: String
    var color: String
    
    init(brand: String, color: String) {
        self.brand = brand
        self.color = color
    }
    
    func accelerate() {
        print("Speeding up...")
    }
}

class Car: Vehicle {
    var numberOfSeats: Int
    
    init(brand: String, color: String, numberOfSeats: Int) {
        self.numberOfSeats = numberOfSeats
        super.init(brand: brand, color: color)
    }
    
    override func accelerate() {
        print("Vroom!")
    }
}
```

In the above example, we have a `Vehicle` superclass that has a `brand` and `color` property, and an `accelerate()` method. The `Car` subclass inherits these properties and methods from the `Vehicle` superclass. The `Car` subclass also has an additional property `numberOfSeats`, which is specific to cars.

## Overriding Methods

In Swift, you can override methods inherited from the superclass in the subclass to provide your own implementation. To override a method, you use the `override` keyword before the method declaration in the subclass.

In the above example, we override the `accelerate()` method in the `Car` subclass to print "Vroom!" instead of the default "Speeding up...".

## Conclusion

Inheritance in Swift Playgrounds allows you to reuse and extend code, promoting code reuse and modularity. Understanding the basics of inheritance and how to override methods gives you the flexibility to create customized subclasses without modifying the original superclass. By leveraging inheritance, you can write clean and efficient code that is easy to maintain and scale.

#Swift #Inheritance #OOP