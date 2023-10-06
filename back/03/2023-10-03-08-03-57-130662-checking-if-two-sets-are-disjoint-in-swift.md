---
layout: post
title: "Checking if two sets are disjoint in Swift"
description: " "
date: 2023-10-03
tags: []
comments: true
share: true
---

## Approach 1: Using the `isDisjoint(with:)` method

Swift provides an `isDisjoint(with:)` method, which returns a Boolean value indicating whether the set has no elements in common with the specified set. We can leverage this method to check if two sets are disjoint or not.

```swift
let set1: Set<Int> = [1, 2, 3, 4]
let set2: Set<Int> = [5, 6, 7, 8]
let set3: Set<Int> = [3, 4, 5, 6]

if set1.isDisjoint(with: set2) {
    print("set1 and set2 are disjoint")
} else {
    print("set1 and set2 have common elements")
}

if set1.isDisjoint(with: set3) {
    print("set1 and set3 are disjoint")
} else {
    print("set1 and set3 have common elements")    
}
```

Output:

```
set1 and set2 are disjoint
set1 and set3 have common elements
```

In the above code snippet, we have three sets: `set1`, `set2`, and `set3`. We use the `isDisjoint(with:)` method to check if `set1` is disjoint with `set2` and `set3` respectively. The output confirms whether the sets are disjoint or have common elements.

## Approach 2: Using the Intersection

Another way to check if two sets are disjoint is to find their intersection using the `intersection(_:)` method. If the resulting intersection set is empty, it means the two sets are disjoint.

```swift
let set1: Set<Int> = [1, 2, 3, 4]
let set2: Set<Int> = [5, 6, 7, 8]
let set3: Set<Int> = [3, 4, 5, 6]

let disjointCheck1 = set1.intersection(set2).isEmpty
let disjointCheck2 = set1.intersection(set3).isEmpty

if disjointCheck1 {
    print("set1 and set2 are disjoint")
} else {
    print("set1 and set2 have common elements")
}

if disjointCheck2 {
    print("set1 and set3 are disjoint")
} else {
    print("set1 and set3 have common elements")    
}
```

Output:

```
set1 and set2 are disjoint
set1 and set3 have common elements
```

In the above code snippet, by taking the intersection of `set1` with `set2` and `set3`, we check if the resulting set is empty. If it is, we conclude that the sets are disjoint; otherwise, they have common elements.

That's it! Now you have two different approaches to check if two sets are disjoint in Swift. Choose the one that suits your requirements and coding style. #Swift #Set