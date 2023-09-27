---
layout: post
title: "Conditional conformance and type inference in Swift"
description: " "
date: 2023-09-27
tags: [programming]
comments: true
share: true
---

Swift is a powerful and expressive programming language that offers various advanced features to enhance developer productivity and code readability. Two of these features are conditional conformance and type inference, which play a crucial role in writing concise and reusable code. In this blog post, we will explore conditional conformance and type inference in Swift and how they can be leveraged to write efficient and elegant code.

## Conditional Conformance

Conditional conformance is a feature in Swift that allows a type to conform to a protocol only under certain conditions, such as when its generic parameters satisfy specific requirements. This feature enables developers to define more precise and specialized behavior for generic types depending on their associated types or constraints.

```swift
protocol Numeric {
    static func +(lhs: Self, rhs: Self) -> Self
}

struct Vector<T: Numeric> {
    let x: T
    let y: T
}

extension Vector: Equatable where T: Equatable {
    static func ==(lhs: Vector<T>, rhs: Vector<T>) -> Bool {
        return lhs.x == rhs.x && lhs.y == rhs.y
    }
}

extension Vector: CustomStringConvertible where T: CustomStringConvertible {
    var description: String {
        return "(\(x), \(y))"
    }
}

let a = Vector(x: 1, y: 2) // Vector<Int>
let b = Vector(x: 3, y: 4) // Vector<Int>
let c = Vector(x: 1.5, y: 2.5) // Vector<Double>

print(a == b) // false
print(a) // (1, 2)
print(c) // (1.5, 2.5)
```

In the above example, the `Vector` struct represents a two-dimensional vector. The conditional conformance allows the `Vector` type to conform to the `Equatable` protocol only when its generic type `T` also conforms to `Equatable`. Similarly, the `Vector` type conditionally conforms to the `CustomStringConvertible` protocol when its generic type `T` conforms to `CustomStringConvertible`. This approach ensures that we can compare and print vectors only if the underlying type supports these operations, improving code safety and readability.

## Type Inference

Type inference is a fundamental feature in Swift that enables the compiler to automatically deduce the type of a variable or expression based on its assigned value or surrounding context. This feature eliminates the need for explicit type annotations, making code more concise, readable, and less prone to errors.

```swift
let number = 42 // inferred as Int
let pi = 3.14 // inferred as Double

let result = number + Int(pi) // inferred as Int
let message = "The result is: \(result)" // inferred as String

var array = [1, 2, 3, 4, 5] // inferred as [Int]
array.append(6) // OK, inferred as Int
// array.append(1.5) // Error: Cannot convert value of type 'Double' to expected argument type 'Int'

func addTwoNumbers(_ a: Int, _ b: Int) -> Int {
    return a + b
}

let sum = addTwoNumbers(number, number) // inferred as Int
```

In the above example, the type inference feature automatically deduces the appropriate types for variables `number`, `pi`, `result`, `message`, `array`, and function parameters (`a` and `b`). Swift infers the variables as `Int`, `Double`, `String`, and `Array` of `Int`, respectively, based on their assigned values or usage context.

Type inference, coupled with conditional conformance, allows developers to write expressive and less verbose code. By leveraging these powerful Swift features, you can enhance your productivity and create more robust and maintainable applications.

#swift #programming