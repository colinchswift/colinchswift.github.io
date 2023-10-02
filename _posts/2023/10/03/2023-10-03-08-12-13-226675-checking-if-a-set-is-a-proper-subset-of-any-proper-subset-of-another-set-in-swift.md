---
layout: post
title: "Checking if a set is a proper subset of any proper subset of another set in Swift"
description: " "
date: 2023-10-03
tags: [swift, subset]
comments: true
share: true
---

In Swift, you can use the `isStrictSubset(of:)` method to check if a set is a proper subset of another set. A proper subset is a subset that is not equal to the original set.

Here's an example code that demonstrates how to check if a set is a proper subset of any proper subset of another set:

```swift
let set1: Set<Int> = [1, 2, 3, 4]
let set2: Set<Int> = [1, 2, 3]

if set2.isStrictSubset(of: set1) {
    print("set2 is a proper subset of set1")
} else {
    print("set2 is not a proper subset of set1")
}

for subset in set1.subsets() {
    if subset.count < set1.count && set2.isStrictSubset(of: subset) {
        print("set2 is a proper subset of", subset)
    }
}
```

In the above code, we have two sets: `set1` and `set2`. We first check if `set2` is a proper subset of `set1` using the `isStrictSubset(of:)` method. If `set2` is a proper subset of `set1`, we print "set2 is a proper subset of set1".

Next, we loop through all the subsets of `set1` using the `subsets()` method. For each subset, we check if it is a proper subset of `set2`. If it is, we print "set2 is a proper subset of" followed by the subset.

This way, we can check if a set is a proper subset of any proper subset of another set in Swift.

#swift #set #subset