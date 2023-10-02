---
layout: post
title: "Sorting a set in descending order in Swift"
description: " "
date: 2023-10-03
tags: [Swift, Sorting]
comments: true
share: true
---

```swift
let mySet: Set<Int> = [5, 1, 3, 2, 4]
let sortedSet = mySet.sorted(by: >)
print(sortedSet) // Output: [5, 4, 3, 2, 1]
```

In the code above, we have a set called `mySet` containing some integers. We then use the `sorted(by:)` function to sort the elements in descending order. The `>` operator is used as the sorting criteria to compare the elements in the set.

Finally, we print the `sortedSet` and observe the output, which will display the integers in descending order.

By utilizing the `sorted(by:)` function along with the `>` operator, you can easily sort a set in descending order in Swift. This approach can be extended to sort other types of collections as well. #Swift #Sorting