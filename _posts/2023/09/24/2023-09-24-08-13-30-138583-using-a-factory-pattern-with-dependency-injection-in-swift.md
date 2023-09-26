---
layout: post
title: "Using a factory pattern with dependency injection in Swift"
description: " "
date: 2023-09-24
tags: [FactoryPattern]
comments: true
share: true
---

In software development, the **Factory Pattern** is a creational design pattern that provides an abstract interface for creating objects. It aims to decouple object creation from the classes that use the objects, allowing for easier testing, maintenance, and scalability. Additionally, the **Dependency Injection** (DI) principle helps manage object dependencies, improving code reusability and modularity.


## The Problem

Imagine that we are building an e-commerce application in Swift, and we have different payment methods: CreditCardPayment and PayPalPayment. Each payment method has its own implementation that conforms to a common PaymentProtocol.


## The Solution

To implement the Factory Pattern with Dependency Injection, we can define a PaymentFactory class that encapsulates the creation logic. This class will have a method that takes a parameter, such as a payment method string, and returns an instance of the appropriate payment method.

Here's an example implementation of the PaymentFactory class in Swift:

```swift
protocol PaymentProtocol {
    func makePayment(amount: Double)
}

class CreditCardPayment: PaymentProtocol {
    func makePayment(amount: Double) {
        // Implementation specific to credit card payment
    }
}

class PayPalPayment: PaymentProtocol {
    func makePayment(amount: Double) {
        // Implementation specific to PayPal payment
    }
}

class PaymentFactory {
    static func createPayment(method: String) -> PaymentProtocol? {
        switch method {
            case "credit_card":
                return CreditCardPayment()
                
            case "paypal":
                return PayPalPayment()
                
            default:
                return nil
        }
    }
}
```

To use the factory, we can simply call the `createPayment` method with the desired payment method string. For example:

```swift
let paymentMethod = PaymentFactory.createPayment(method: "credit_card")
paymentMethod?.makePayment(amount: 100.0)
```

This code will create an instance of the `CreditCardPayment` class and call the `makePayment` method on it. We can easily switch to a different payment method by passing the corresponding method string to the `createPayment` method.


## The Benefits

By using the Factory Pattern with Dependency Injection, we achieve the following benefits:

1. **Decoupling**: The client code doesn't need to know the exact implementation details of each payment method. It only relies on the common `PaymentProtocol` interface, allowing for easier code maintenance and updates.

2. **Flexibility**: Adding or removing payment methods becomes easier, as we only need to modify the factory class. The client code remains unaffected by these changes.

3. **Testability**: We can easily write unit tests for the client code by stubbing or mocking the payment method objects. This is possible because the factory creates instances based on an abstract interface.

#swift #FactoryPattern #DependencyInjection