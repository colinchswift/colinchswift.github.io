---
layout: post
title: "Strategy pattern with functional programming in Swift"
description: " "
date: 2023-10-26
tags: []
comments: true
share: true
---

In object-oriented programming, the Strategy pattern allows you to define a family of algorithms, encapsulate each one as a separate class, and make them interchangeable. This pattern helps in decoupling the implementation of an algorithm from the code using it.

With Swift's functional programming capabilities like higher-order functions and closures, we can take advantage of functional concepts to implement the Strategy pattern in a more concise and expressive way.

## The Scenario

Let's say we have a `PaymentProcessor` class that processes payments. However, the payment processing logic may vary depending on the payment method, such as credit card, PayPal, or Apple Pay. We want to be able to switch between these different strategies without modifying the `PaymentProcessor` class.

## Functional Strategy Pattern Implementation

We can start by defining a protocol for the payment strategy:

```swift
protocol PaymentStrategy {
    func processPayment(amount: Double)
}
```

Next, we can create separate structs or classes for each payment method, conforming to the `PaymentStrategy` protocol:

```swift
struct CreditCardStrategy: PaymentStrategy {
    func processPayment(amount: Double) {
        // Logic to process payment via credit card
    }
}

struct PayPalStrategy: PaymentStrategy {
    func processPayment(amount: Double) {
        // Logic to process payment via PayPal
    }
}

struct ApplePayStrategy: PaymentStrategy {
    func processPayment(amount: Double) {
        // Logic to process payment via Apple Pay
    }
}
```

Now, let's create a `PaymentProcessor` class that takes a payment strategy as a parameter and processes the payment using that strategy:

```swift
class PaymentProcessor {
    private let paymentStrategy: PaymentStrategy
    
    init(paymentStrategy: PaymentStrategy) {
        self.paymentStrategy = paymentStrategy
    }
    
    func processPayment(amount: Double) {
        paymentStrategy.processPayment(amount: amount)
    }
}
```

Finally, we can use the payment processor by passing in the desired payment strategy:

```swift
let creditCardStrategy = CreditCardStrategy()
let paymentProcessor = PaymentProcessor(paymentStrategy: creditCardStrategy)
paymentProcessor.processPayment(amount: 100.0)
```

## Benefits of Functional Strategy Pattern

By using functional programming concepts, we achieve the following benefits:

- **Improved code reusability**: Each payment strategy is a separate class or struct that can be reused in multiple contexts.
- **Easier to add new strategies**: Adding new payment strategies is as simple as creating a new class or struct conforming to the `PaymentStrategy` protocol without modifying existing code.
- **Decoupling of payment processing logic**: The `PaymentProcessor` class is decoupled from the specific payment strategy implementation, making it more maintainable and easier to test.

## Conclusion

In this blog post, we explored how to implement the Strategy pattern using functional programming concepts in Swift. By leveraging Swift's functional capabilities, we can achieve a more concise, expressive, and maintainable codebase. The functional Strategy pattern allows us to easily switch between different strategies without modifying existing code, improving code reusability and maintainability.