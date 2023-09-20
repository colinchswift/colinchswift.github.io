---
layout: post
title: "Examples of using generics in Swift"
description: " "
date: 2023-09-20
tags: [SwiftGenerics, SwiftProgramming]
comments: true
share: true
---

Generics in Swift allow us to write flexible and reusable functions, structures, and classes that can work with any type. They provide a way to define placeholders for types that are then specified when the code is used.

Here are a few examples that demonstrate how to use generics in Swift:

## 1. Generic Functions

A generic function can work with different types of parameters. Here's an example of a generic function that swaps two values:

```swift
func swapValues<T>(_ a: inout T, _ b: inout T) {
    let temp = a
    a = b
    b = temp
}

var num1 = 5
var num2 = 10
swapValues(&num1, &num2)
// Now num1 is 10 and num2 is 5

var name1 = "John"
var name2 = "Jane"
swapValues(&name1, &name2)
// Now name1 is "Jane" and name2 is "John"
```

In the above example, the function `swapValues` is generic and can swap values of any type `T`.

## 2. Generic Types

We can also create generic structures and classes. Here's an example of a generic stack data structure:

```swift
struct Stack<Element> {
    private var elements = [Element]()
    
    mutating func push(_ element: Element) {
        elements.append(element)
    }
    
    mutating func pop() -> Element? {
        return elements.popLast()
    }
}

var stack = Stack<Int>()
stack.push(1)
stack.push(2)
stack.push(3)
stack.pop() // Returns 3
stack.pop() // Returns 2
```

In the above example, the `Stack` structure is generic and can be used with any element type.

## Conclusion

Generics in Swift provide a powerful mechanism for writing flexible and reusable code. They allow us to create functions, structures, and classes that can work with different types. By leveraging generics, we can write code that is more generic, flexible, and efficient.

#SwiftGenerics #SwiftProgramming