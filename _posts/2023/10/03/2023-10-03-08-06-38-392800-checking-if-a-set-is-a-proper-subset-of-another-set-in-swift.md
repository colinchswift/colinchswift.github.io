---
layout: post
title: "Checking if a set is a proper subset of another set in Swift"
description: " "
date: 2023-10-03
tags: [Swift, Sets]
comments: true
share: true
---

Let's consider two sets, `setA` and `setB`, and test whether `setA` is a proper subset of `setB`.

```swift
let setA: Set<Int> = [1, 2, 3]
let setB: Set<Int> = [1, 2, 3, 4, 5]

let isProperSubset = setA.isStrictSubset(of: setB) // true
```
In the above code, we have defined two sets, `setA` and `setB`, with `setA` being the potential proper subset and `setB` being the superset.

To check if `setA` is a proper subset of `setB`, we use the `isStrictSubset(of:)` method on the set. This method returns `true` if all the elements in `setA` are present in `setB` but `setB` also contains additional elements.

After running the code, the `isProperSubset` variable will contain `true` since all elements from `setA` (1, 2, and 3) are present in `setB` and `setB` also contains additional elements (4 and 5).

If we were to modify `setA` to include an element that is not present in `setB`, the result would be `false`.

```swift
let setA: Set<Int> = [1, 2, 3, 6]  // Adding 6 to setA
let setB: Set<Int> = [1, 2, 3, 4, 5]

let isProperSubset = setA.isStrictSubset(of: setB) // false
```

In the updated code, the `isProperSubset` variable will now contain `false` since `setA` contains an element (6) that is not present in `setB`.

Using the `isStrictSubset(of:)` method allows you to easily check if one set is a proper subset of another set in Swift. It provides a simple and efficient way to perform set comparison operations in your code. 

So the next time you need to check for set relationships, give this functionality a try in your Swift projects!

#Swift  #Sets