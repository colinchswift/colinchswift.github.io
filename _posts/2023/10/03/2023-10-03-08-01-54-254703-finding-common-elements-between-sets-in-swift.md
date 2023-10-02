---
layout: post
title: "Finding common elements between sets in Swift"
description: " "
date: 2023-10-03
tags: [swift, common]
comments: true
share: true
---

When working with sets in Swift, you might come across situations where you need to find the common elements across multiple sets. Fortunately, Swift provides a simple and efficient way to achieve this.

## Using the `intersection` method

The most straightforward way to find the common elements between sets in Swift is by using the `intersection` method provided by the `Set` type. This method returns a new set that contains the common elements shared between two sets.

Here's an example code snippet that demonstrates how to use the `intersection` method:

```swift
let firstSet: Set<Int> = [1, 2, 3, 4, 5]
let secondSet: Set<Int> = [4, 5, 6, 7, 8]

let commonElements = firstSet.intersection(secondSet)
print(commonElements) // Output: [4, 5]
```

In this example, we have two sets `firstSet` and `secondSet`. By calling the `intersection` method on `firstSet` and passing `secondSet` as the argument, we get a new set `commonElements` that contains the common elements `[4, 5]` between the two sets.

## Using the `isDisjoint` method

Another useful method provided by the `Set` type is `isDisjoint`, which can be used to check if two sets have any common elements. This method returns a boolean value indicating whether the sets have any elements in common.

Here's an example code snippet that demonstrates how to use the `isDisjoint` method:

```swift
let firstSet: Set<Int> = [1, 2, 3, 4, 5]
let secondSet: Set<Int> = [6, 7, 8, 9, 10]

if firstSet.isDisjoint(with: secondSet) {
    print("The sets have no common elements")
} else {
    print("The sets have common elements")
}
```

In this example, we have two sets `firstSet` and `secondSet`. By calling the `isDisjoint` method on `firstSet` and passing `secondSet` as the argument, we can determine if the sets have any common elements. If they don't, "The sets have no common elements" will be printed; otherwise, "The sets have common elements" will be printed.

#swift #set #common-elements