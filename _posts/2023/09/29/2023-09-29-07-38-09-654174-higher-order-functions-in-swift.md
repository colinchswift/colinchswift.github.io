---
layout: post
title: "Higher-Order Functions in Swift"
description: " "
date: 2023-09-29
tags: [higherorderfunctions]
comments: true
share: true
---

In Swift, higher-order functions are an essential part of functional programming. These functions take in other functions as parameters or return functions as their results. They allow you to abstract over actions, making your code more concise and expressive.

## Map

`map` is a higher-order function in Swift that applies a given closure expression to each element of a collection and returns an array with the transformed elements in the same order.

```swift
let numbers = [1, 2, 3, 4, 5]
let doubled = numbers.map { $0 * 2 }
// doubled: [2, 4, 6, 8, 10]
```

In the example above, the closure expression `{$0 * 2}` is applied to each element of the `numbers` array, doubling each element and creating a new array `doubled`.

## Filter

`filter` is another higher-order function that takes a closure expression as a parameter. It evaluates the closure for each element in a collection and returns a new array with elements that satisfy the given condition.

```swift
let numbers = [1, 2, 3, 4, 5]
let evenNumbers = numbers.filter { $0 % 2 == 0 }
// evenNumbers: [2, 4]
```

Here, the closure expression `{ $0 % 2 == 0 }` is used to filter out only the even numbers from the `numbers` array, resulting in a new array `evenNumbers`.

## Reduce

`reduce` is a powerful higher-order function that combines all elements of a collection into a single value using a closure expression. It takes an initial value and repeatedly applies the closure expression to the current accumulated value and each element of the collection.

```swift
let numbers = [1, 2, 3, 4, 5]
let sum = numbers.reduce(0) { $0 + $1 }
// sum: 15
```

In this case, the closure expression `{ $0 + $1 }` is used to calculate the sum of all elements in the `numbers` array, starting from an initial value of 0.

## Conclusion

Higher-order functions like `map`, `filter`, and `reduce` are powerful tools in Swift that allow you to write more concise and expressive code. They enable you to abstract over common operations on collections, making your code easier to read and maintain.

By leveraging these higher-order functions, you can take advantage of Swift's functional programming capabilities and write code that is both efficient and elegant.

#swift #higherorderfunctions