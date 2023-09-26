---
layout: post
title: "Access Control in Swift Properties"
description: " "
date: 2023-09-22
tags: [AccessControl]
comments: true
share: true
---

Access control is an important aspect of modern programming languages, including Swift. It allows developers to define and restrict the visibility and modification of properties, methods, and other members within a module or across modules.

In this blog post, we will focus on access control in Swift properties, exploring the different access levels available and their usage scenarios.

## Access Levels

Swift offers five different access levels for properties:

1. **Open**: The highest level of access, allowing entities to be accessed and subclassed from any module, including the defining module and any modules importing it.

2. **Public**: Allows entities to be accessed from any module, including the defining module and any modules importing it, but prevents subclassing and overriding outside the defining module.

3. **Internal**: The default access level, making entities accessible within the defining module but not outside of it. This access level is useful for properties that are only needed internally within a module.

4. **File-private**: Limits the access to the current source file. Any properties marked as file-private can only be accessed from within the same source file. This level of access enhances encapsulation by restricting visibility to a single file.

5. **Private**: The most restrictive access level, confining properties to the enclosing declaration. With this level of access, properties can only be accessed within the same class, struct, or extension.

## Usage and Examples

Let's consider some scenarios to understand how access control in Swift properties can be utilized effectively.

**1. Publicly Accessible Properties:**
```swift
public class Customer {
    public var name: String
    public let id: Int
    
    public init(name: String, id: Int) {
        self.name = name
        self.id = id
    }
}
```
In this example, we have a publicly accessible class `Customer` with two properties: `name` and `id`. These properties can be accessed from any module, making them suitable for public APIs.

**2. Internal Properties:**
```swift
internal struct Employee {
    var name: String
    var department: String
    
    // internal initializer
    init(name: String, department: String) {
        self.name = name
        self.department = department
    }
}
```
In this code snippet, we define an internal struct `Employee` with `name` and `department` properties. As the access level is internal, it is accessible within the same module only.

**3. Encapsulated Properties:**
```swift
class BankAccount {
    private var balance: Double
    
    init(balance: Double) {
        self.balance = balance
    }
    
    public func getBalance() -> Double {
        return balance
    }
    
    public func deposit(amount: Double) {
        balance += amount
    }
    
    public func withdraw(amount: Double) {
        if amount <= balance {
            balance -= amount
        } else {
            print("Insufficient balance!")
        }
    }
}
```
In this example, we have a private property `balance` inside the `BankAccount` class. The property is encapsulated, meaning it cannot be accessed or modified outside the class. Instead, we provide public methods to get the balance, deposit, and withdraw funds, ensuring controlled access and preventing direct manipulation of the balance.

## Conclusion

Access control in Swift properties allows developers to define the visibility and modification of properties, ensuring encapsulation and protecting sensitive data. Choosing the appropriate access level for properties depends on the specific needs of the module or class. By leveraging access control effectively, developers can create maintainable and secure Swift codebases.

#Swift #AccessControl #Properties