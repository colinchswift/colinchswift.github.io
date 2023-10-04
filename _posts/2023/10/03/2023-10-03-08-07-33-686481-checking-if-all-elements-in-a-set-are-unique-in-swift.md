---
layout: post
title: "Checking if all elements in a set are unique in Swift"
description: " "
date: 2023-10-03
tags: [Uniqueness]
comments: true
share: true
---

To check if all elements in a set are unique, you can compare the count of the set with the count of an array created from the set. If the counts match, it indicates that all the elements in the set are unique.

Here's an example in Swift:

```swift
let mySet: Set<Int> = [1, 2, 3, 4, 5]
let isUnique = mySet.count == Array(mySet).count
print("All elements are unique: \(isUnique)")
```

In this example, we define a set called `mySet` containing integers. We then create an array from the set using the `Array(mySet)` syntax. Finally, we compare the count of the set with the count of the array and store the result in the `isUnique` variable. The `print` statement displays whether all elements in the set are unique.

Remember to replace the `Int` type with the appropriate type for your use case. This method works for any type that conforms to the `Hashable` protocol, such as `String`, `Double`, or custom types.

With this approach, you can easily check if all elements in a set are unique in Swift, ensuring data integrity and avoiding duplicates.

\#Swift \#Set \#Uniqueness