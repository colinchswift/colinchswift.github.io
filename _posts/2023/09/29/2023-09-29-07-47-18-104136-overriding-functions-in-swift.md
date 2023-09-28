---
layout: post
title: "Overriding Functions in Swift"
description: " "
date: 2023-09-29
tags: [swift, functions]
comments: true
share: true
---

In object-oriented programming, the concept of "overriding" allows a subclass to provide its own implementation of a method that is already implemented in its superclass. This enables the subclass to customize the behavior of the method while still inheriting other properties and methods from the superclass.

## Understanding Function Override

To override a function in Swift, you declare a new function in the subclass with the same name, parameters, and return type as the original function in the superclass. You use the `override` keyword to indicate that you are intentionally overriding the method.

Here's an example to illustrate the concept:

```swift
class Vehicle {
    func accelerate() {
        print("The vehicle is accelerating.")
    }
}

class Car: Vehicle {
    override func accelerate() {
        print("The car is accelerating.")
    }
}

let myCar = Car()
myCar.accelerate() // Output: "The car is accelerating."
```

In the example above, the `Vehicle` class has a function called `accelerate()`, which simply prints a message. The `Car` class, which is a subclass of `Vehicle`, overrides the `accelerate()` function with its own implementation. When we create an instance of `Car` and call the `accelerate()` function, it prints a different message defined in the subclass.

## Superclass Invocation

When overriding a function, you can still access the original implementation of the superclass using the `super` keyword. This is useful when you want to extend or modify the behavior of the superclass.

```swift
class Vehicle {
    func accelerate() {
        print("The vehicle is accelerating.")
    }
}

class Car: Vehicle {
    override func accelerate() {
        super.accelerate() // Invoke the superclass implementation
        print("The car is accelerating faster.")
    }
}

let myCar = Car()
myCar.accelerate()
```

In this example, the `Car` class invokes the `accelerate()` function of the superclass `Vehicle` using `super.accelerate()` before printing its own message. This allows the `Car` class to augment the behavior of the inherited function.

## Conclusion

Overriding functions in Swift is a powerful feature that allows subclasses to modify the behavior of methods defined in their superclass. By understanding how to correctly override functions and use the `super` keyword, you can effectively customize the behavior of your classes while maintaining code reusability and inheritance.

#swift #functions #inheritance