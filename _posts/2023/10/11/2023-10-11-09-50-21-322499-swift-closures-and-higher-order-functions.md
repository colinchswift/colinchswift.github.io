---
layout: post
title: "Swift closures and higher-order functions"
description: " "
date: 2023-10-11
tags: [Closures, HigherOrderFunctions]
comments: true
share: true
---

In Swift, closures are self-contained blocks of code that can be assigned to variables, passed around as arguments to functions, or returned from functions. They are similar to anonymous functions or lambdas in other programming languages. Closures are powerful and versatile, and they become even more useful when combined with higher-order functions.

## What are Closures?

In Swift, closures are defined using the `{}` braces. They can capture and store references to variables and constants from the surrounding context in which they are defined. Closures can take parameters and return values, just like regular functions. Here's an example of a closure in Swift:

```swift
let greet = { (name: String) in
    print("Hello, \(name)!")
}

greet("John") // Output: Hello, John!
```

In the above example, we define a closure called `greet` that takes a `String` parameter `name` and prints a greeting message. We then call the closure by passing it the argument "John".

## Higher-Order Functions

Higher-order functions are functions that can take one or more functions as arguments or return a function as a result. Swift provides a set of built-in higher-order functions that operate on collections, such as arrays and dictionaries. These higher-order functions allow for concise and expressive code.

### Map

The `map` function applies a given closure to each element of a collection and returns a new collection containing the transformed elements. Here's an example that doubles every element of an array using `map`:

```swift
let numbers = [1, 2, 3, 4, 5]
let doubledNumbers = numbers.map { $0 * 2 } // [2, 4, 6, 8, 10]
```

In the above example, we use the `map` function to transform each element of the `numbers` array by multiplying it by 2. The resulting `doubledNumbers` array contains the transformed elements.

### Filter

The `filter` function creates a new collection that contains only the elements of the original collection that satisfy a given condition. Here's an example that filters out all the odd numbers from an array:

```swift
let numbers = [1, 2, 3, 4, 5]
let evenNumbers = numbers.filter { $0 % 2 == 0 } // [2, 4]
```

In the above example, the `filter` function is used to select only the even numbers from the `numbers` array based on the condition `$0 % 2 == 0`.

### Reduce

The `reduce` function combines all elements of a collection into a single value by applying a combining closure. It takes an initial value and a closure that specifies how to combine each element with the result so far. Here's an example that calculates the sum of all elements in an array:

```swift
let numbers = [1, 2, 3, 4, 5]
let sum = numbers.reduce(0) { $0 + $1 } // 15
```

In the above example, the `reduce` function starts with an initial value of 0 and adds each element of the `numbers` array to the accumulated sum using the closure `$0 + $1`.

## Conclusion

Swift closures and higher-order functions allow for more concise and expressive code. Closures provide a way to define self-contained blocks of code, and higher-order functions enable us to perform powerful transformations and operations on collections. By mastering closures and higher-order functions, you can write cleaner and more efficient Swift code.

\#Swift #Closures #HigherOrderFunctions