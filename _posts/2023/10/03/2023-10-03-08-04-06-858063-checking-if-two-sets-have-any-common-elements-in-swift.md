---
layout: post
title: "Checking if two sets have any common elements in Swift"
description: " "
date: 2023-10-03
tags: [programming, swift]
comments: true
share: true
---

### Approach 1: Intersection

The easiest way to check if two sets have any common elements is by using the `intersection` method provided by the Set class in Swift. This method returns a new set that contains the common elements between the two sets.

```swift
let set1: Set<Int> = [1, 2, 3, 4, 5]
let set2: Set<Int> = [4, 5, 6, 7, 8]

let commonElements = set1.intersection(set2)

if commonElements.isEmpty {
    print("The sets have no common elements")
} else {
    print("The sets have common elements: \(commonElements)")
}
```

In the example above, the `intersection` method is called on `set1` with `set2` as the parameter. The result is stored in the `commonElements` set. If the `commonElements` set is empty, it means that the sets have no common elements.

### Approach 2: Checking element existence

Another approach is to iterate over one set and check if each element exists in the other set. This can be done using the `contains` method provided by the Set class.

```swift
let set1: Set<Int> = [1, 2, 3, 4, 5]
let set2: Set<Int> = [4, 5, 6, 7, 8]

var hasCommonElements = false

for element in set1 {
    if set2.contains(element) {
        hasCommonElements = true
        break
    }
}

if hasCommonElements {
    print("The sets have common elements")
} else {
    print("The sets have no common elements")
}
```

In this example, we iterate over each element in `set1` and check if it exists in `set2` using the `contains` method. If a common element is found, we set the `hasCommonElements` flag to `true` and exit the loop.

Both approaches work efficiently in checking if two sets have any common elements in Swift. Choose the one that suits your coding style and requirement.

#programming #swift