---
layout: post
title: "Checking if two sets have any common elements in Swift"
description: " "
date: 2023-10-03
tags: [Swift, CommonElements]
comments: true
share: true
---

Method 1: Using the `intersection` method
---------------------------------------

The `intersection` method is provided by the `Set` type in Swift, which returns a new set containing the common elements between two sets.

```swift
let set1: Set<Int> = [1, 2, 3, 4, 5]
let set2: Set<Int> = [4, 5, 6, 7, 8]

let commonElements = set1.intersection(set2)

if commonElements.isEmpty {
    print("The sets have no common elements.")
} else {
    print("The sets have common elements: \(commonElements)")
}
```

In this example, `set1` and `set2` are two sets containing numbers. We use the `intersection` method to find the common elements between the sets. If the resulting set is empty, it means the sets have no common elements. Otherwise, we print the common elements.

Method 2: Using the `isDisjoint` method
--------------------------------------

The `isDisjoint` method is another way to check whether two sets have any common elements. It returns a boolean value indicating whether the sets have any common elements.

```swift
let set1: Set<Int> = [1, 2, 3, 4, 5]
let set2: Set<Int> = [6, 7, 8]

if set1.isDisjoint(with: set2) {
    print("The sets have no common elements.")
} else {
    print("The sets have common elements.")
}
```

In this example, we use the `isDisjoint` method to check whether `set1` and `set2` have any common elements. If the method returns `true`, it means the sets have no common elements. Otherwise, we print that the sets have common elements.

Conclusion
----------

In this blog post, we've explored two methods to check whether two sets have any common elements in Swift. The `intersection` method returns a new set containing the common elements, while the `isDisjoint` method returns a boolean value indicating whether the sets have any common elements.

#Swift #Set #CommonElements