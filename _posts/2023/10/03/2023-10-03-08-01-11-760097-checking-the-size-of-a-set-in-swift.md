---
layout: post
title: "Checking the size of a set in Swift"
description: " "
date: 2023-10-03
tags: [swift, size]
comments: true
share: true
---

Here is an example of how you can check the size of a set in Swift:

```swift
let fruits: Set<String> = ["Apple", "Banana", "Orange", "Mango"]
let size = fruits.count
print("The size of the set is \(size)") // The size of the set is 4
```

In the above code, we have a set called `fruits` that contains four elements. We use the `count` property to get the size of the set and store it in the `size` constant. Finally, we print the size using string interpolation.

Remember, in Swift, sets are unordered collections of unique values, so you don't have to worry about duplicate elements. If you ever need to check the size of a set, the `count` property will give you the desired information.

#swift #set #size