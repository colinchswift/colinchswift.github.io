---
layout: post
title: "Applying CompactMap Functions in Swift"
description: " "
date: 2023-09-29
tags: [swift, higherorderfunctions]
comments: true
share: true
---

If you are a Swift developer, you may have come across the `compactMap` function. This powerful method allows you to transform the elements of a collection while simultaneously removing any resulting `nil` values.

## How does `compactMap` work?

The `compactMap` function is a higher-order function that takes a closure as its parameter. It iterates over the elements of the collection and applies the closure to each element. The result is an array of non-`nil` transformed values.

Here's an example to illustrate its usage:

```swift
let numbers = ["1", "2", "3", "four", "5"]

let parsedNumbers = numbers.compactMap { Int($0) }

print(parsedNumbers) // Output: [1, 2, 3, 5]
```

In this example, we have an array of strings representing numbers. We use the `compactMap` function with a closure that attempts to convert each string element into an `Int`. The resulting array will only contain the valid converted values, discarding the `"four"` string.

## Handling optional values with `compactMap`

`compactMap` is particularly useful when dealing with optional values. It allows you to transform optional values and discard any resulting `nil` values conveniently. This can be handy when working with APIs or other scenarios where you need to transform data.

Here's an example of working with optional values:

```swift
let optionalNumbers: [Int?] = [1, 2, nil, 3, nil, 4]

let unwrappedNumbers = optionalNumbers.compactMap { $0 }

print(unwrappedNumbers) // Output: [1, 2, 3, 4]
```

In this case, we have an array of optional integers. Using `compactMap` with the identity closure (`{ $0 }`) filters out the `nil` values, leaving us with an array of unwrapped integers.

## Conclusion

The `compactMap` function in Swift is a powerful tool for transforming collections and handling optional values. By applying a closure to each element, you can easily filter out `nil` values and obtain a new array containing only the non-`nil` transformed values. Whether you're parsing data or dealing with optionals, `compactMap` can streamline your code and make it more concise.

#swift #higherorderfunctions