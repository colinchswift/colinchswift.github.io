---
layout: post
title: "Type constraints in Swift generics"
description: " "
date: 2023-09-20
tags: [swift, generics]
comments: true
share: true
---

## Basic Generic Function

Let's start by looking at a basic generic function:

```swift
func swapValues<T>(_ a: inout T, _ b: inout T) {
    let temp = a
    a = b
    b = temp
}
```

The above function `swapValues` swaps the values of two variables of the same type `T`. The type parameter `T` is a placeholder for a specific type that will be determined when the function is called.

## Type Constraints

Sometimes, you need to restrict the types that can be used with generics to a certain subset. Swift provides the ability to specify type constraints on a generic type. There are various ways to apply type constraints, as described below.

### 1. Type Inheritance

You can apply a type constraint to a generic type by specifying that it should inherit from a certain class or conform to a particular protocol. Here's an example:

```swift
func printName<T: Person>(_ person: T) {
    print(person.name)
}
```

In the above code, the generic type `T` is constrained to be of type `Person` or any class that inherits from `Person`. This allows the function to access the `name` property, which is defined in the `Person` class.

### 2. Protocol Conformance

You can also constrain a generic type to conform to one or more protocols. This allows you to access the properties and methods defined in those protocols within the generic function. Here's an example:

```swift
func processObject<T: Codable>(_ object: T) {
    let data = try? JSONEncoder().encode(object)
    // Process the encoded data
}
```

In the above code, the generic type `T` is constrained to conform to the `Codable` protocol. This allows the function to encode the `object` using `JSONEncoder`, as the `Codable` protocol guarantees the object's ability to be encoded.

## Where Clauses

In addition to specifying type constraints directly in the generic type declaration, you can use the `where` clause to provide more complex constraints. This allows you to check for additional conditions or define relationships between multiple generic types. Here's an example:

```swift
func processValues<T, U>(_ value1: T, _ value2: U) where T: Numeric, U: Comparable {
    if value1 > value2 {
        print("Value 1 is greater")
    } else {
        print("Value 2 is greater or equal")
    }
}
```

In the above code, the `where` clause is used to specify that `T` must be a numeric type and `U` must be a comparable type. This allows the function to compare the values using the `>` and `<=` operators.

## Conclusion

Type constraints in Swift generics provide a flexible way to define generic functions, structures, and classes that work with a specified subset of types. Whether it's through type inheritance, protocol conformance, or the use of where clauses, you have the power to define the behavior and restrictions for your generics. By leveraging these type constraints effectively, you can write more robust and efficient code that is reusable across different types.

#swift #generics