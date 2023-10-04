---
layout: post
title: "Applying Sorting Functions in Swift"
description: " "
date: 2023-09-29
tags: [Sorting]
comments: true
share: true
---

Sorting is a fundamental operation in any programming language. In Swift, there are several built-in sorting functions that make it easy to sort arrays and collections. In this article, we will explore how to apply these sorting functions in Swift.

## Sort an Array

To sort an array in Swift, you can use the `sort()` function. Here's an example:

```swift
var numbers = [5, 2, 7, 9, 1]
numbers.sort()
print(numbers) // Output: [1, 2, 5, 7, 9]
```

In this example, the `sort()` function sorts the `numbers` array in ascending order. If you want to sort the array in descending order, you can use the `sort(by:)` function and pass a closure that defines the sorting logic. Here's an example:

```swift
var numbers = [5, 2, 7, 9, 1]
numbers.sort { (a, b) -> Bool in
    return a > b
}
print(numbers) // Output: [9, 7, 5, 2, 1]
```

## Sort a Collection

Swift provides a `sort(by:)` function that can be used to sort any collection. The `sort(by:)` function takes a closure that defines the sorting logic. Here's an example:

```swift
var fruits = ["banana", "apple", "orange", "grape"]
let sortedFruits = fruits.sorted { (a, b) -> Bool in
    return a < b
}
print(sortedFruits) // Output: ["apple", "banana", "grape", "orange"]
```

In this example, the `sorted(by:)` function sorts the `fruits` collection in ascending order based on the alphabetical order of the elements.

## Sort with Custom Objects

If you have a custom object and want to sort an array or collection of those objects, you need to conform to the `Comparable` protocol. The `Comparable` protocol provides default implementations for the less-than (`<`) and equal-to (`==`) operators, which are required for sorting. Here's an example:

```swift
struct Person: Comparable {
    let name: String
    let age: Int
    
    static func <(lhs: Person, rhs: Person) -> Bool {
        return lhs.age < rhs.age
    }
}

var people = [Person(name: "John", age: 25), Person(name: "Alice", age: 30), Person(name: "Bob", age: 20)]
people.sort()
print(people) // Output: [Person(name: "Bob", age: 20), Person(name: "John", age: 25), Person(name: "Alice", age: 30)]
```

In this example, the `Person` struct conforms to the `Comparable` protocol by implementing the less-than operator (`<`) based on the age property. The `people` array is then sorted using the default sorting function.

## Conclusion

Sorting is a crucial operation in many programming tasks. With Swift's built-in sorting functions, you can easily sort arrays and collections of any type. Whether you want to sort in ascending or descending order or sort custom objects, Swift provides flexible and convenient sorting options.

#Swift #Sorting