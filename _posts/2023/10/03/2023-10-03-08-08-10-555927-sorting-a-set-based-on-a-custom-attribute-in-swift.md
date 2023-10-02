---
layout: post
title: "Sorting a set based on a custom attribute in Swift"
description: " "
date: 2023-10-03
tags: [Swift, Sorting]
comments: true
share: true
---

In Swift, a `Set` is an unordered collection of unique values. However, there may be scenarios where you need to sort a set based on a specific attribute or property of its elements. In this article, we will explore how to sort a set based on a custom attribute.

Let's say you have a `Person` struct as follows:

```swift
struct Person {
    let name: String
    let age: Int
}
```

Now, imagine you have a set of `Person` objects:

```swift
var peopleSet: Set<Person> = [
    Person(name: "John", age: 25),
    Person(name: "Alice", age: 30),
    Person(name: "Emma", age: 20)
]
```

To sort the `peopleSet` based on the `age` attribute, you can convert it to an array, sort the array using the desired attribute, and then convert it back to a set.

```swift
let sortedPeopleArray = peopleSet.sorted { $0.age < $1.age }
let sortedPeopleSet = Set(sortedPeopleArray)
```

In this example, we're using the `sorted(by:)` function, which takes a closure to specify the sorting order. The closure compares two `Person` objects based on their `age` attribute in ascending order.

Now, `sortedPeopleSet` will be a set with the `Person` objects sorted by age.

## Conclusion

Sorting a set based on a custom attribute in Swift involves converting the set to an array, sorting the array using the desired attribute, and then converting it back to a set. This technique allows you to achieve the desired order for your set elements.

#Swift #Sorting