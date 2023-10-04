---
layout: post
title: "Applying Sorted Functions in Swift"
description: " "
date: 2023-09-29
tags: [Sorting]
comments: true
share: true
---

Sorting data is a common task in programming, and Swift provides several built-in functions to help us sort arrays and collections. In this blog post, we will explore how to use the `sorted()` function and its variations to sort data in Swift.

### The `sorted()` Function

The `sorted()` function is a high-order function in Swift that takes an array or collection and returns a new sorted array or collection. 

Here is an example of how to use the `sorted()` function to sort an array of integers in ascending order:

```swift
let numbers = [9, 5, 2, 7, 1]
let sortedNumbers = numbers.sorted()
print(sortedNumbers)
```
Output:
```
[1, 2, 5, 7, 9]
```

By default, the `sorted()` function sorts the elements in ascending order. But what if we want to sort them in descending order?

### Sorting in Descending Order

To sort an array in descending order, we can use the `sorted(by:)` function and provide a closure that specifies the sorting order. 

For example, if we want to sort an array of strings in descending order of length, we can do it like this:

```swift
let fruits = ["apple", "banana", "orange", "kiwi"]
let sortedFruits = fruits.sorted(by: { $0.count > $1.count })
print(sortedFruits)
```
Output:
```
["banana", "orange", "apple", "kiwi"]
```

In this example, we use the closure `{ $0.count > $1.count }` to compare the lengths of two strings. If the length of the first string `$0` is greater than the length of the second string `$1`, the closure returns `true` and the elements are sorted accordingly.

### Sorting Custom Objects

Sorting custom objects in Swift requires a way to compare the objects. We can achieve this by implementing the `Comparable` protocol and providing an implementation for the `>` operator.

Here is an example of how to sort an array of `Person` objects based on their ages:

```swift
struct Person: Comparable {
    let name: String
    let age: Int
    
    static func < (lhs: Person, rhs: Person) -> Bool {
        return lhs.age < rhs.age
    }
}

let people = [
    Person(name: "John", age: 30),
    Person(name: "Jane", age: 25),
    Person(name: "Tom", age: 35)
]

let sortedPeople = people.sorted()
print(sortedPeople.map { $0.name })
```
Output:
```
["Jane", "John", "Tom"]
```

In this example, we define a `Person` struct that conforms to the `Comparable` protocol by implementing the `<` operator to compare ages. Then, we can simply use the `sorted()` function to sort an array of `Person` objects.

### Conclusion

Sorting data is an important feature in any programming language, and Swift provides powerful built-in functions to handle this task. With the `sorted()` function and its variations, we can easily sort arrays and collections in both ascending and descending order. Whether it's sorting primitive types or custom objects, Swift has got us covered.

\#Swift \#Sorting