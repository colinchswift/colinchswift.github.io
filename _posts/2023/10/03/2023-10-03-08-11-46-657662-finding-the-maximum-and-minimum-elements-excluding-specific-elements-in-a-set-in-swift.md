---
layout: post
title: "Finding the maximum and minimum elements excluding specific elements in a set in Swift"
description: " "
date: 2023-10-03
tags: []
comments: true
share: true
---

In Swift, you can easily find the maximum and minimum elements in a set using built-in functions. However, there may be cases where you want to exclude specific elements from consideration. In this blog post, we will explore how to find the maximum and minimum elements while excluding specific elements in a set in Swift.

Let's say we have a set of integers and we want to find the maximum and minimum values excluding the numbers that are divisible by 3. Here's how we can do it:

```swift
let numbers: Set<Int> = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

let filteredNumbers = numbers.filter { $0 % 3 != 0 }
let maximum = filteredNumbers.max()
let minimum = filteredNumbers.min()

print("Maximum: \(maximum ?? 0)") // Output: Maximum: 10
print("Minimum: \(minimum ?? 0)") // Output: Minimum: 1
```

In the code snippet above, we first create a set called `numbers` with integer values. We then use the `filter` function to create a new set called `filteredNumbers` by excluding the numbers in `numbers` that are divisible by 3. We use a closure to define the filtering condition (`$0 % 3 != 0` checks if a number is not divisible by 3).

Finally, we use the `max` and `min` functions on `filteredNumbers` to find the maximum and minimum values respectively. If the set is empty, these functions will return `nil`, so we provide a default value of 0 using the nil coalescing operator (`??`) in the print statements.

#### Conclusion

In this blog post, we explored how to find the maximum and minimum elements in a set while excluding specific elements in Swift. By using the `filter` function, we can easily create a new set without the excluded elements and then find the maximum and minimum values using the `max` and `min` functions respectively.