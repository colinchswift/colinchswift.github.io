---
layout: post
title: "Checking if a set contains elements satisfying multiple conditions in Swift"
description: " "
date: 2023-10-03
tags: [Swift, SetOperations]
comments: true
share: true
---

Let's say we have a set of integers and we want to check if it contains any elements that are both greater than 10 and divisible by 2. Here's an example code:

```swift
let numbers: Set<Int> = [7, 12, 18, 3, 5]

let containsElements = numbers.contains { number in
    return number > 10 && number % 2 == 0
}

if containsElements {
    print("The set contains elements that satisfy the conditions.")
} else {
    print("The set does not contain elements that satisfy the conditions.")
}
```

In this example, the closure passed to `contains(where:)` checks if each element in the set is greater than 10 and divisible by 2. If at least one element satisfies both conditions, the `containsElements` variable will be `true`. Otherwise, it will be `false`.

By utilizing the powerful closure capabilities of Swift, you can easily check if a set contains elements satisfying multiple conditions. This approach can be applied to other data types as well, making your code more efficient and concise. #Swift #SetOperations