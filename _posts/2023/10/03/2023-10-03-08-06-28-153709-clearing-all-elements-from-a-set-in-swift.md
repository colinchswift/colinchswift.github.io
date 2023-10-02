---
layout: post
title: "Clearing all elements from a set in Swift"
description: " "
date: 2023-10-03
tags: [SwiftProgramming, SetOperations]
comments: true
share: true
---

```swift
var set: Set<Int> = [1, 2, 3, 4, 5]

// Clear all elements from the set
set.removeAll()

print(set) // Output: []
```

In the above example, we declare a set called `set` and initialize it with some elements. To clear all elements from the set, we simply call the `removeAll()` method on the `set` variable. This will remove all elements from the set, leaving it empty.

Finally, we print the set to verify that it is indeed empty. The output will be an empty set `[]`.

It's important to note that `removeAll()` is an O(n) operation, where n is the number of elements in the set. So, if you have a large set, be aware that clearing all elements may take some time.

Remember to check the official Swift documentation for more information on sets and their methods.

#SwiftProgramming #SetOperations