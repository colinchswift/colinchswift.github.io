---
layout: post
title: "Creating a set from a sequence in Swift"
description: " "
date: 2023-10-03
tags: [Swift, Sets]
comments: true
share: true
---

Here's an example code snippet that demonstrates how to create a set from a sequence in Swift:

```swift
let numbers = [1, 2, 3, 4, 5, 5, 4, 3, 2, 1]
let numberSet = Set(numbers)

print(numberSet) // Output: [2, 3, 5, 4, 1]
```

In this example, we have an array of numbers called `numbers`. We then create a new set called `numberSet` using the `Set` initializer and passing in the `numbers` array. This will remove any duplicates from the array and create a set with unique values.

After creating the set, we can print it out and see the result. In this case, the output will be `[2, 3, 5, 4, 1]`, as the duplicates have been removed.

Using the `Set` initializer is a convenient way to convert a sequence into a set in Swift. It works with any sequence type, not just arrays. So, whether you have an array, a range, or any other sequence, you can easily create a set from it using this approach.

#Swift #Sets