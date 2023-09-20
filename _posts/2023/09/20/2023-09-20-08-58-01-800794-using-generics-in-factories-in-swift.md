---
layout: post
title: "Using generics in factories in Swift"
description: " "
date: 2023-09-20
tags: [Swift, Generics]
comments: true
share: true
---

When it comes to creating objects in Swift, factories are a commonly used design pattern. By using factories, you can encapsulate the creation logic of an object and hide it from the client code, providing a more flexible and maintainable solution. 

However, sometimes you may need to create objects of different types based on certain conditions or input parameters. This is where generics come in handy. Generics allow you to create factories that can dynamically return objects of different types without sacrificing type safety. 

To demonstrate the usage of generics in factories, let's consider an example scenario where we have a `Vehicle` protocol and several concrete types that conform to it.

```swift
protocol Vehicle {
    func startEngine()
    func stopEngine()
}

struct Car: Vehicle {
    func startEngine() {
        print("Car engine started")
    }
    
    func stopEngine() {
        print("Car engine stopped")
    }
}

struct Motorcycle: Vehicle {
    func startEngine() {
        print("Motorcycle engine started")
    }
    
    func stopEngine() {
        print("Motorcycle engine stopped")
    }
}
```

Now, let's create a generic `VehicleFactory` that can create instances of any type that conforms to the `Vehicle` protocol. The factory will take a type parameter `T` which represents the specific type of object to create.

```swift
class VehicleFactory {
    
    static func create<T: Vehicle>() -> T {
        return T()
    }
    
}
```

Using the `create` method of the `VehicleFactory`, we can now create objects of different types without knowing the specific type beforehand. The type parameter `T` is inferred based on the return type of the method.

```swift
let car = VehicleFactory.create() as Car
car.startEngine() // Output: Car engine started

let motorcycle = VehicleFactory.create() as Motorcycle
motorcycle.startEngine() // Output: Motorcycle engine started
```

In the above example, we created instances of both `Car` and `Motorcycle` using the same factory method `create()`. This demonstrates the flexibility of using generics in factories. 

Additionally, by using generics, you ensure type safety at compile-time. The factory method only allows objects of types that conform to the `Vehicle` protocol to be created.

In conclusion, using generics in factories in Swift can provide a powerful and flexible way to create objects of different types based on specific conditions. It allows you to encapsulate the creation logic and maintain type safety at compile-time. Incorporating generics in your factory designs can result in cleaner and more maintainable code.

#Swift #Generics