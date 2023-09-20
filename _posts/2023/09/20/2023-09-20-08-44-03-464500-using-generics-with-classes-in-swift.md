---
layout: post
title: "Using generics with classes in Swift"
description: " "
date: 2023-09-20
tags: [Generics]
comments: true
share: true
---

Generics are a powerful feature in Swift that allow you to write reusable code that works with different types. They enable you to create flexible and type-safe classes, functions, and structures. In this blog post, we will explore how to use generics with classes in Swift.

## Declaring a Generic Class

To declare a generic class in Swift, you need to use the `class` keyword followed by the class name, and then specify the generic type parameter within angle brackets `< >`. The type parameter can be named anything you want and is used as a placeholder for the actual type that will be specified when creating an instance of the class.

Here's an example of a generic class called `Stack` that can hold elements of any type:

```swift
class Stack<T> {
    private var elements = [T]()

    func push(_ element: T) {
        elements.append(element)
    }

    func pop() -> T? {
        return elements.popLast()
    }
}
```
In this example, the `Stack` class has a single type parameter `T` which represents the type of elements stored in the stack. The `push` method adds an element to the top of the stack, and the `pop` method removes and returns the top element.

## Creating Instances of a Generic Class

When creating an instance of a generic class, you need to specify the actual type for the generic parameter. You can do this by providing the type in angle brackets after the class name.

Here's an example of creating instances of the `Stack` class with different types:

```swift
let stringStack = Stack<String>()
stringStack.push("Apple")
stringStack.push("Banana")

let intStack = Stack<Int>()
intStack.push(10)
intStack.push(20)
```

In this example, `stringStack` is an instance of `Stack` that holds strings, while `intStack` is an instance of `Stack` that holds integers. You can create as many instances of `Stack` as you need, each with a different type.

## Type Constraints

Swift also allows you to provide type constraints on generic parameters. This means you can restrict the types that can be used with a generic class to a specific set of types or to types that conform to certain protocols.

Here's an example of a generic class `MathOperation` that performs a mathematical operation on its generic type parameter `T`, but only if `T` conforms to the `Numeric` protocol:

```swift
protocol Numeric {
    static func +(lhs: Self, rhs: Self) -> Self
}

class MathOperation<T: Numeric> {
    func add(lhs: T, rhs: T) -> T {
        return lhs + rhs
    }
}
```

In this example, the `MathOperation` class specifies a type constraint `T: Numeric` which ensures that the generic type parameter `T` must conform to the `Numeric` protocol. The `add` method performs an addition operation on two values of type `T` using the `+` operator.

## Conclusion

Generics provide a flexible and type-safe way to write reusable code in Swift. Using generics with classes allows you to create generic containers and perform operations on generic types without sacrificing type safety. By leveraging generics, you can write more efficient and reusable code in your Swift projects.

#Swift #Generics #Classes