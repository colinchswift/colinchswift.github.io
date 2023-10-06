---
layout: post
title: "Checking if a set is equal to an array in Swift"
description: " "
date: 2023-10-03
tags: [Array]
comments: true
share: true
---

## Approach 1: Converting Set to Array

One straightforward approach is to convert the Set to an Array and then compare the Arrays. Here is an example code snippet:

```swift
let set: Set<Int> = [1, 2, 3, 4, 5]
let array = [1, 2, 3, 4, 5]

if set.sorted() == array.sorted() {
    print("The set is equal to the array.")
} else {
    print("The set is not equal to the array.")
}
```

In this approach, we use the `sorted()` method to ensure that the order of elements is the same in both the Set and the Array. If the sorted Arrays are equal, then the Set is equal to the Array.

> #Swift #Set #Array

## Approach 2: Iterating through Elements

Another approach to check if a Set is equal to an Array is by iterating through each element and checking if it exists in both collections. Here is an example code snippet:

```swift
let set: Set<Int> = [1, 2, 3, 4, 5]
let array = [1, 2, 3, 4, 5]

var isEqual = true

for element in set {
    if !array.contains(element) {
        isEqual = false
        break
    }
}

if isEqual && set.count == array.count {
    print("The set is equal to the array.")
} else {
    print("The set is not equal to the array.")
}
```

In this approach, we use a Boolean variable `isEqual` to keep track of whether the elements in the Set exist in the Array. We also check if the count of elements in both collections is the same to ensure they are equal.

> #Swift #Set #Array