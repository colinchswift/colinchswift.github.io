---
layout: post
title: "Checking if a set is a superset and subset of another set in Swift"
description: " "
date: 2023-10-03
tags: [Swift, Sets]
comments: true
share: true
---

In Swift, Sets are a powerful data structure that allows you to store unique values. Sometimes, you may need to check if one set is a superset or subset of another set. In this article, we will explore how to perform these checks in Swift.

## Superset Check:
To check if a set is a superset of another set, you can use the `isSuperset(of:)` method. This method returns a boolean value indicating whether the set is a superset of the specified set.

Here's an example code snippet demonstrating the superset check:

```swift
let setA: Set<Int> = [1, 2, 3, 4, 5]
let setB: Set<Int> = [1, 2, 3]

if setA.isSuperset(of: setB) {
    print("setA is a superset of setB")
} else {
    print("setA is not a superset of setB")
}
```
Output:
```
setA is a superset of setB
```

In the above example, `setA` contains all the elements present in `setB`, making it a superset.

## Subset Check:
To check if a set is a subset of another set, you can use the `isSubset(of:)` method. This method returns a boolean value indicating whether the set is a subset of the specified set.

Here's an example code snippet demonstrating the subset check:

```swift
let setA: Set<Int> = [1, 2, 3]
let setB: Set<Int> = [1, 2, 3, 4, 5]

if setA.isSubset(of: setB) {
    print("setA is a subset of setB")
} else {
    print("setA is not a subset of setB")
}
```
Output:
```
setA is a subset of setB
```

In the above example, `setA` is a subset of `setB` since all the elements in `setA` are present in `setB`.

## Conclusion:
By using the `isSuperset(of:)` and `isSubset(of:)` methods, you can easily check if one set is a superset or subset of another set in Swift. These methods provide a convenient way to perform set comparisons in your Swift applications.

#Swift #Sets