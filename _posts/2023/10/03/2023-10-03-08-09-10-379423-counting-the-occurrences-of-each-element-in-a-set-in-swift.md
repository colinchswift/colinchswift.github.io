---
layout: post
title: "Counting the occurrences of each element in a set in Swift"
description: " "
date: 2023-10-03
tags: [Swift, Frequency]
comments: true
share: true
---

Here's an example code snippet to illustrate how to count the occurrences of each element in a set:

```swift
let fruits: Set<String> = ["apple", "banana", "orange", "apple", "grape", "banana", "apple"]

let frequency = fruits.reduce(into: [:]) { (counts, fruit) in
    counts[fruit, default: 0] += 1
}

print(frequency)
```

This code snippet creates a set called `fruits` with some duplicate elements. We then use the `reduce(into:)` method on the `fruits` set, initializing an empty dictionary as the initial value.

Inside the closure, we update the count for each fruit by accessing the value in the `counts` dictionary using the fruit as the key. If the fruit is not yet in the dictionary, we use the `default` parameter to set the initial value to 0. We then increment the count by 1.

Finally, we print the `frequency` dictionary, which contains the count for each fruit:

```swift
["banana": 2, "orange": 1, "grape": 1, "apple": 3]
```

By utilizing the `reduce(into:)` method and a dictionary, we can easily count the occurrences of each element in a set in Swift.

#Swift #Set #Frequency #Count