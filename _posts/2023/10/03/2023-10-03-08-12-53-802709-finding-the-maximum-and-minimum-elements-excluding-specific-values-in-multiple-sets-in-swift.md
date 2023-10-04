---
layout: post
title: "Finding the maximum and minimum elements excluding specific values in multiple sets in Swift"
description: " "
date: 2023-10-03
tags: [DataAnalysis]
comments: true
share: true
---

When working with multiple sets of data in Swift, it can be useful to find the maximum and minimum elements while excluding specific values. This can be done using a combination of filtering and the built-in `max()` and `min()` functions.

Let's dive into an example to illustrate how to achieve this.

## Example

Assume we have three sets of numbers: `set1`, `set2`, and `set3`. We want to find the maximum and minimum values from each set, excluding the value `0`. Here's the code to achieve this:

```swift
let set1: Set<Int> = [1, 2, 3, 0, 5]
let set2: Set<Int> = [4, 0, 6, 7, 8]
let set3: Set<Int> = [9, 0, 10, 11, 12]

// Filtering out 0 values and finding the maximum and minimum elements
let max1 = set1.filter { $0 != 0 }.max()
let min1 = set1.filter { $0 != 0 }.min()

let max2 = set2.filter { $0 != 0 }.max()
let min2 = set2.filter { $0 != 0 }.min()

let max3 = set3.filter { $0 != 0 }.max()
let min3 = set3.filter { $0 != 0 }.min()

print("Max and Min of Set 1: \(max1 ?? 0), \(min1 ?? 0)")
print("Max and Min of Set 2: \(max2 ?? 0), \(min2 ?? 0)")
print("Max and Min of Set 3: \(max3 ?? 0), \(min3 ?? 0)")
```

In the above code, we use the `filter()` method to exclude the value `0` from each set. Then, we use the `max()` and `min()` functions to find the maximum and minimum elements from the filtered sets. Finally, we print out the results for each set.

## Conclusion

By using a combination of filtering and the `max()` and `min()` functions in Swift, it is straightforward to find the maximum and minimum elements from multiple sets while excluding specific values. This can be useful in various scenarios where you need to identify extreme values in your data. #Swift #DataAnalysis