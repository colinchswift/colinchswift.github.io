---
layout: post
title: "Checking if a set contains another set in Swift"
description: " "
date: 2023-10-03
tags: []
comments: true
share: true
---

Here's an example that demonstrates how to check if one set is a subset of another set in Swift:

```swift
let superSet: Set<Int> = [1, 2, 3, 4, 5]
let subSet: Set<Int> = [2, 3]

if subSet.isSubset(of: superSet) {
    print("subSet is a subset of superSet")
} else {
    print("subSet is not a subset of superSet")
}
```

In this example, we have a `superSet` that contains the numbers 1, 2, 3, 4, and 5, and a `subSet` that contains the numbers 2 and 3. The `.isSubset(of:)` method is called on the `subSet`, passing in the `superSet` as the argument. If the result is true, we print "subSet is a subset of superSet". Otherwise, we print "subSet is not a subset of superSet".

Remember, both the `superSet` and `subSet` must be of the same type for the `.isSubset(of:)` method to work correctly.