---
layout: post
title: "Checking if two sets are equal in Swift"
description: " "
date: 2023-10-03
tags: []
comments: true
share: true
---

To demonstrate this, let's consider two sets: `setA` and `setB`. We want to check if these sets contain the same elements.

```swift
let setA: Set<String> = ["apple", "banana", "orange"]
let setB: Set<String> = ["banana", "orange", "apple"]

if setA == setB {
    print("Sets are equal.")
} else {
    print("Sets are not equal.")
}
```

In the above example, the order of elements does not matter when comparing sets for equality. The `==` operator checks if the two sets have the same elements, regardless of their order.

If you run the code, it will output "Sets are equal." because `setA` and `setB` contain the same elements, albeit in a different order.

Remember to use the appropriate type for your sets (`Int`, `String`, etc.) when working with sets in Swift.