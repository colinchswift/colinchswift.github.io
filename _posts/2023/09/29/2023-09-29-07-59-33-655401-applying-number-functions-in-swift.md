---
layout: post
title: "Applying Number Functions in Swift"
description: " "
date: 2023-09-29
tags: [NumberFunctions]
comments: true
share: true
---

When working with numbers in Swift, you have access to a range of built-in number functions that can simplify your code and enable you to perform various mathematical operations. In this blog post, we will explore some of the most commonly used number functions in Swift and how to apply them in your code.

## 1. Absolute Value

The absolute value function in Swift, `abs()`, returns the absolute value of a number. It disregards the sign and always returns a non-negative value. For example:

```swift
let num = -9
let absolute = abs(num)
print(absolute) // Output: 9
```

## 2. Rounding Numbers

Swift provides several functions for rounding numbers to a specified decimal place. The most commonly used functions are `round()`, `ceil()`, and `floor()`.

- `round()`: Round a number to the nearest whole number.

```swift
let num1 = 3.49
let rounded1 = round(num1)
print(rounded1) // Output: 3

let num2 = 4.51
let rounded2 = round(num2)
print(rounded2) // Output: 5
```

- `ceil()`: Round a number to the next highest whole number.

```swift
let num = 2.7
let roundedUp = ceil(num)
print(roundedUp) // Output: 3
```

- `floor()`: Round a number to the next lowest whole number.

```swift
let num = 2.7
let roundedDown = floor(num)
print(roundedDown) // Output: 2
```

## 3. Converting between Degrees and Radians

Swift includes functions to convert between degrees and radians, namely `degrees(from:)` and `radians(from:)`.

- `degrees(from:)`: Converts radians to degrees.

```swift
import Foundation

let radianValue: Double = .pi/2 // 90 degrees in radians
let degreeValue = degrees(from: radianValue)
print(degreeValue) // Output: 90
```

- `radians(from:)`: Converts degrees to radians.

```swift
import Foundation

let degreeValue = 180 // 180 degrees in radians
let radianValue = radians(from: degreeValue)
print(radianValue) // Output: 3.14159
```

## Conclusion

In this blog post, we explored some of the most commonly used number functions in Swift. These powerful functions allow you to manipulate numbers, round them, and convert between degrees and radians. By familiarizing yourself with these functions, you can write cleaner and more efficient code when working with numbers in Swift.

#Swift #NumberFunctions #SwiftProgramming