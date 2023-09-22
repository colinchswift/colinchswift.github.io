---
layout: post
title: "Access Control in Swift Encapsulation vs Abstraction"
description: " "
date: 2023-09-22
tags: [Swift, AccessControl]
comments: true
share: true
---

When it comes to designing software systems, there are two important concepts in object-oriented programming: **encapsulation** and **abstraction**. In the context of Swift, encapsulation refers to the idea of hiding implementation details of a class or struct, while abstraction refers to the process of simplifying complex systems by creating more generalized and reusable components. In this blog post, we will explore how these concepts relate to access control in Swift.

## Access Levels in Swift

Swift provides five different access levels to control the visibility and accessibility of code entities like classes, structs, variables, and functions. These access levels are:

1. **Open**: The highest access level, allowing entities to be used from any source module, including other modules that import the module containing the open entity. Typically used when designing a framework's public API.

2. **Public**: Allows entities to be used from any source module but restricts subclassing or overriding outside the source module. Used for public APIs of frameworks.

3. **Internal**: The default access level, accessible within the same module. Not available outside the module, making it suitable for code that should not be exposed as part of the module's public API.

4. **File-private**: Restricts the use of an entity to its own defining source file. This level is useful for hiding implementation details within a single file and is not accessible from other files within the same module.

5. **Private**: The most restrictive access level, allowing access only within the enclosing declaration. Use this level to hide implementation details within a specific class or struct.

It's important to choose the appropriate access level for each code entity to strike a balance between encapsulation and abstraction.

## Encapsulation and Access Control

Encapsulation is all about information hiding and maintaining the integrity of an object's data. By encapsulating properties and methods, we can prevent direct access to internal data and provide a controlled interface for interacting with the object.

In Swift, we can achieve encapsulation by defining properties and methods with an access level of **private** or **file-private**. With these access levels, the internal implementation details are hidden from other parts of the codebase, ensuring data integrity and preventing unwanted modifications.

For example, consider a class representing a bank account:

```swift
class BankAccount {
    private var balance: Double

    init(initialBalance: Double) {
        self.balance = initialBalance
    }

    func deposit(amount: Double) {
        balance += amount
    }

    func withdraw(amount: Double) {
        if amount <= balance {
            balance -= amount
        } else {
            print("Insufficient balance")
        }
    }
}
```

In this example, the `balance` property is declared as private, ensuring that it can only be accessed and modified within the `BankAccount` class. The `deposit` and `withdraw` methods provide a controlled interface for interacting with the account's balance, enforcing encapsulation.

## Abstraction and Access Control

Abstraction allows us to hide complex implementation details and provide a simplified, high-level interface for using a system. It involves creating reusable components that represent a higher-level concept, without exposing the underlying complexity.

In Swift, abstraction can be achieved through access levels such as **public** and **open**. By exposing only the necessary information and functionality through public APIs, we can abstract away the inner workings of a module or framework.

For example, consider a framework for handling network requests:

```swift
public struct NetworkRequest {
    public static func send(url: URL) {
        // Implementation details
    }
}

public protocol NetworkDelegate: AnyObject {
    func didReceiveData(data: Data)    
}
```

In this example, the `NetworkRequest` struct provides a public API for sending network requests without exposing the internal implementation details. The `NetworkDelegate` protocol defines a high-level interface for handling received data, abstracting away the details of network communication.

## Conclusion

Access control in Swift allows us to achieve both encapsulation and abstraction, key principles in object-oriented programming. By carefully choosing the appropriate access levels for different code entities, we can strike a balance between hiding implementation details and providing a simplified interface for using our systems. So remember to consider access control when designing your Swift codebase, ensuring a clean and modular structure.

#Swift #AccessControl #Encapsulation #Abstraction