---
layout: post
title: "Using generics in property wrappers in Swift"
description: " "
date: 2023-09-20
tags: []
comments: true
share: true
---

Swift provides property wrappers as a powerful feature to implement custom behavior for properties. Property wrappers are a great way to encapsulate common property logic, such as validation or transformation, into reusable components.

Generics, on the other hand, allow us to write code that can work with different types, providing flexibility and reusability. Combining property wrappers with generics allows us to create highly customizable and reusable code.

## Definition of a Property Wrapper

Before diving into generics, let's quickly review the definition of a property wrapper. Property wrappers wrap around a property, intercepting its access and enabling additional behavior.

To create a property wrapper, you need to declare a structure or class with a `wrappedValue` property, along with optional `projectedValue` and `init(wrappedValue:projectedValue:)` members.

Here's an example of a property wrapper that adds validation on an integer property:

```swift
@propertyWrapper
struct ValidatedInt {
    private var value: Int = 0
    
    var wrappedValue: Int {
        get { value }
        set {
            if newValue > 0 {
                value = newValue
            } else {
                print("Invalid value")
            }
        }
    }
    
    init(wrappedValue: Int) {
        self.wrappedValue = wrappedValue
    }
}
```

## Using Generics with Property Wrappers

By default, property wrappers work with a specific type. However, with the help of generics, we can make property wrappers work with any type.

To create a generic property wrapper, we need to add a type parameter to the property wrapper struct or class. This type parameter can then be used in the `wrappedValue` property and the initialization logic.

Let's see an example of a generic property wrapper that adds validation on a generic type:

```swift
@propertyWrapper
struct Validated<Value> {
    private var value: Value?
    
    var wrappedValue: Value? {
        get { value }
        set {
            if let newValue = newValue {
                // Custom validation logic
                if isValid(value: newValue) {
                    value = newValue
                } else {
                    print("Invalid value")
                }
            } else {
                value = nil
            }
        }
    }
    
    init(wrappedValue: Value?) {
        self.wrappedValue = wrappedValue
    }
    
    private func isValid(value: Value) -> Bool {
        // Custom validation logic
        // Return true if value is valid, otherwise false
        // ...
        return true
    }
}
```

In the above example, the `Validated` property wrapper can now work with any type. The validation logic in `wrappedValue` can be customized for different types by implementing the `isValid` method.

## Usage

To use the generic property wrapper, simply annotate your properties with the wrapper's name, followed by angle brackets and the desired type parameter:

```swift
struct User {
    @Validated<Int> var age: Int?
    @Validated<String> var name: String?
}
```

In this example, the `age` property is validated using the `Validated` property wrapper with an `Int` type parameter, while the `name` property is validated using the same property wrapper with a `String` type parameter.

## Summary

By combining property wrappers with generics in Swift, we can create flexible and reusable code. Generic property wrappers allow us to define custom behavior for properties that can work with any type, providing great flexibility and code reuse.