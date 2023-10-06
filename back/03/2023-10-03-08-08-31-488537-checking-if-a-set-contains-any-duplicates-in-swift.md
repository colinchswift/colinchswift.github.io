---
layout: post
title: "Checking if a set contains any duplicates in Swift"
description: " "
date: 2023-10-03
tags: []
comments: true
share: true
---

Here's an example code snippet to check for duplicates in a set:

```swift
let set: Set<Int> = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
let array = Array(set)

let hasDuplicates = set.count != array.count

if hasDuplicates {
    print("The set contains duplicates")
} else {
    print("The set does not contain duplicates")
}
```

In the code above, we first create a set called `set` with some integer values. Then, we convert the set into an array using the `Array()` initializer. After that, we compare the count of the set with the count of the array using the `!=` operator. If the counts are not equal, it means the set contains duplicates.

You can run this code in a Swift playground or in a Swift file in your Xcode project to test it out.