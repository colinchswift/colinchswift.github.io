---
layout: post
title: "Functions and methods in Swift"
description: " "
date: 2023-10-01
tags: [SwiftProgramming, FunctionsAndMethods]
comments: true
share: true
---

In Swift, functions are blocks of reusable code that can be called to perform a specific task. Methods, on the other hand, are functions that are associated with a particular class, structure, or enumeration. In this blog post, we will explore functions and methods in Swift and understand how they can be used in your code.

## Functions

A function in Swift is defined using the `func` keyword, followed by the function name and a set of parentheses that may contain a list of input parameters. Here's an example of a simple function that takes two integers as input and returns their sum:

```swift
func sum(_ a: Int, _ b: Int) -> Int {
    return a + b
}
```

In the above example, the function `sum` takes two integer parameters `a` and `b` and returns their sum as an `Int`. The underscore `_` before the parameter names allows us to call the function without explicitly mentioning the parameter names.

We can call the `sum` function like this:

```swift
let result = sum(5, 3) // result = 8
```

We can also define functions that have no return value by using the `Void` type or simply omitting the return type. Here's an example:

```swift
func greet(_ name: String) {
    print("Hello, \(name)!")
}

greet("John") // prints "Hello, John!"
```

## Methods

Methods are functions associated with a particular type, such as a class, structure, or enumeration. They are defined inside the type's body and can access the properties and other methods of that type. Here's an example of a method defined inside a struct:

```swift
struct Rectangle {
    var width: Double
    var height: Double
    
    func area() -> Double {
        return width * height
    }
}

let rectangle = Rectangle(width: 5, height: 3)
let area = rectangle.area() // area = 15.0
```

In the above example, the struct `Rectangle` has a method called `area()` that calculates and returns the area of the rectangle.

Methods can also have mutating keyword `mutating` in front of them if they need to modify the properties of the struct or enum. Here's an example:

```swift
struct Counter {
    var count = 0
    
    mutating func increment() {
        count += 1
    }
}

var counter = Counter()
counter.increment() // count = 1
```

In the above example, the `increment()` method modifies the value of the `count` property, so it has to be marked as `mutating`.

## Conclusion

In this blog post, we explored functions and methods in Swift. Functions are reusable blocks of code that can be called to perform a specific task, while methods are functions associated with a particular type. Understanding how to use functions and methods will greatly enhance your ability to write clean and efficient code in Swift. #SwiftProgramming #FunctionsAndMethods