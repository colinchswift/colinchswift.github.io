---
layout: post
title: "Type parameters in Swift generics"
description: " "
date: 2023-09-20
tags: [Generics]
comments: true
share: true
---

To define a type parameter in Swift, we use angle brackets ("<" and ">") followed by a name inside the angle brackets. For example, let's say we want to create a generic function that swaps two values:

```swift
func swapValues<T>(_ a: inout T, _ b: inout T) {
    let temp = a
    a = b
    b = temp
}
```

In the above code, the letter "T" is used as the type parameter. It can be replaced with any type when the function is called.

When calling a generic function, we specify the types explicitly or let Swift infer the types based on the arguments passed. For example:

```swift
var a = 5
var b = 10

print("Before swapping: a = \(a), b = \(b)")

swapValues(&a, &b)

print("After swapping: a = \(a), b = \(b)")
```

In this example, the type parameter "T" is inferred based on the types of the arguments passed (`Int` in this case).

Using type parameters in Swift generics allows us to write reusable code that can work with different types. It provides flexibility and type safety at the same time.

#Swift #Generics