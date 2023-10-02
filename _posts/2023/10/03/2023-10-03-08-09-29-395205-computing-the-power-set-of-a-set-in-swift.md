---
layout: post
title: "Computing the power set of a set in Swift"
description: " "
date: 2023-10-03
tags: [Swift, PowerSet]
comments: true
share: true
---

```swift
func powerSet<T>(set: [T]) -> [[T]] {
    let n = set.count
    var powerSet = [[T]]()

    // Loop through all possible combinations using binary representation
    for i in 0..<(1<<n) {
        var subset = [T]()

        // Check each bit of the binary representation
        for j in 0..<n {
            if (i & (1<<j)) != 0 {
                subset.append(set[j])
            }
        }

        powerSet.append(subset)
    }

    return powerSet
}
```

Here's a breakdown of how the code works:

1. We define a function called `powerSet` that takes an input set of any generic type `T` and returns an array of arrays.

2. We initialize an empty array called `powerSet` to store the subsets of the input set.

3. We use a loop to iterate through all possible combinations of the set. The loop variable `i` represents the binary representation of each combination.

4. Within the loop, we initialize an empty array called `subset` to store each subset.

5. We use another loop to check each bit of the binary representation. If the bit at position `j` is set (i.e., equals 1), we add the corresponding element from the input set to the `subset` array.

6. After the inner loop completes, we append the `subset` array to the `powerSet` array.

7. Finally, we return the `powerSet` array containing all possible subsets of the input set.

To use this function, you can simply call it with an array of elements, like this:

```swift
let set = [1, 2, 3]
let result = powerSet(set: set)
print(result)
```

This will output the power set of the input set `[1, 2, 3]`:

```
[[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3]]
```

By computing the power set of a set, you can perform tasks such as generating all possible combinations or subsets, which can be useful in various algorithms and problem-solving scenarios. #Swift #PowerSet