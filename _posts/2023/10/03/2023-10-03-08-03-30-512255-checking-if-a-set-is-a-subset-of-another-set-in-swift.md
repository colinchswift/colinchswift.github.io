---
layout: post
title: "Checking if a set is a subset of another set in Swift"
description: " "
date: 2023-10-03
tags: [Sets]
comments: true
share: true
---

When working with sets in Swift, it's often necessary to check if one set is a subset of another set. In other words, we want to determine if all the elements in one set are present in another set.

To accomplish this in Swift, we can use the `isSubset(of:)` method provided by the `Set` class. This method returns a boolean value indicating whether the calling set is a subset of the set passed as an argument.

Here's an example of how to use the `isSubset(of:)` method in Swift:

```swift
let set1: Set<Int> = [1, 2, 3, 4, 5]
let set2: Set<Int> = [2, 4, 5]

if set2.isSubset(of: set1) {
    print("set2 is a subset of set1")
} else {
    print("set2 is not a subset of set1")
}
```

In the code above, we define two sets: `set1` and `set2`. We then use the `isSubset(of:)` method to check if `set2` is a subset of `set1`. If the method returns `true`, we print "set2 is a subset of set1". Otherwise, we print "set2 is not a subset of set1".

With this example, we can easily determine if one set contains all the elements of another set in Swift.

# #Swift #Sets