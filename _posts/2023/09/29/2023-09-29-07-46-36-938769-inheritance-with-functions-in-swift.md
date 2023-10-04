---
layout: post
title: "Inheritance with Functions in Swift"
description: " "
date: 2023-09-29
tags: [TechBlog]
comments: true
share: true
---

In object-oriented programming, inheritance allows a class to inherit properties and methods from another class. In Swift, this concept applies not only to properties and methods, but also to functions.

## Basics of Inheritance in Swift

Before diving into inheritance with functions, let's briefly review the basics of inheritance in Swift.

1. **Superclass and Subclass**: In Swift, we have the concept of a superclass and subclass. The superclass is the existing class that provides the blueprint for other related classes, which are referred to as subclasses.

2. **Inheriting Properties and Methods**: A subclass can inherit properties and methods from its superclass. This means that the subclass automatically has access to those properties and methods without needing to redefine them.

3. **Adding Additional Functionality**: In addition to inheriting properties and methods, a subclass can also add its own unique properties and methods.

## Inheritance with Functions

When it comes to functions, Swift provides two types of inheritance: **overriding** and **overloading**.

### Overriding Functions

**Overriding** allows a subclass to provide its own implementation of a function that is already defined in its superclass. Let's take a look at an example:

```swift
class Vehicle {
    func makeSound() {
        print("The vehicle makes a sound!")
    }
}

class Car: Vehicle {
    override func makeSound() {
        print("The car makes a vroom sound!")
    }
}

let myCar = Car()
myCar.makeSound() // Output: The car makes a vroom sound!
```

In this example, the `Car` class inherits the `makeSound()` function from its superclass `Vehicle`. However, the `Car` class overrides the default implementation and provides its own implementation to make a customized sound.

### Overloading Functions

**Overloading** allows a class to have multiple functions with the same name but different parameters. This provides flexibility and allows the class to handle different scenarios based on the parameters passed. Here's an example:

```swift
class MathOperations {
    func add(_ x: Int, _ y: Int) -> Int {
        return x + y
    }
    
    func add(_ x: Int, _ y: Int, _ z: Int) -> Int {
        return x + y + z
    }
}

let math = MathOperations()
print(math.add(2, 3))           // Output: 5
print(math.add(2, 3, 4))        // Output: 9
```

In this example, the `MathOperations` class has two **overloaded** versions of the `add()` function. One accepts two parameters and returns their sum, while the other accepts three parameters and returns their sum.

## Conclusion

Inheritance allows for code reuse and promotes a more organized and modular code structure. With Swift's support for inheritance with functions, you can customize and extend functionality from existing classes to suit your specific needs.

#TechBlog #Swift