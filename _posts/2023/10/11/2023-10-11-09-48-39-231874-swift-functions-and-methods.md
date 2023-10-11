---
layout: post
title: "Swift functions and methods"
description: " "
date: 2023-10-11
tags: [Functions]
comments: true
share: true
---

In Swift, functions and methods are fundamental building blocks of code that allow you to perform specific tasks or execute a piece of code. Both functions and methods encapsulate a set of instructions and can take input parameters to produce an output.

## 1. Swift Functions

A function in Swift is a self-contained block of code that performs a specific task. It can take zero or more parameters as input and can also return a value. Functions enable code reusability and maintainability by allowing you to define a set of instructions that can be called multiple times from different parts of your program.

### Defining a Function

```swift
func greet(name: String) {
    print("Hello, \(name)!")
}
```

In the above example, we define a function called `greet` that takes a single parameter `name` of type `String`. The function body prints a greeting message with the provided `name`.

### Calling a Function

```swift
greet(name: "John")
```

To call a function, you simply use its name followed by parentheses. Pass the required arguments inside the parentheses. In this case, we call the `greet` function and provide the argument `"John"`.

## 2. Swift Methods

Methods, similar to functions, are blocks of code that perform a specific task. The key difference is that methods are associated with a particular type (classes, structures, or enumerations) and are called on instances of that type.

### Defining a Method

```swift
struct Calculator {
    func add(_ a: Int, _ b: Int) -> Int {
        return a + b
    }
}
```

In the above example, we define a method called `add` inside a `Calculator` struct. The method takes two integer parameters `a` and `b` and returns their sum.

### Calling a Method

```swift
let calc = Calculator()
let result = calc.add(5, 7)
print(result) // Output: 12
```

To call a method, you first create an instance of the type it belongs to (`Calculator` in this case). Then, you call the method using dot notation on that instance, passing the required arguments. The returned value can be stored in a variable or used directly.

## Conclusion

Functions and methods are crucial elements in Swift programming, enabling you to encapsulate logic and perform specific tasks. By understanding how to define and call functions and methods, you have the foundation to create reusable and modular code.

I hope this article has provided a clear understanding of Swift functions and methods. Feel free to explore more advanced features and concepts to level up your Swift programming skills.

**#Swift #Functions**