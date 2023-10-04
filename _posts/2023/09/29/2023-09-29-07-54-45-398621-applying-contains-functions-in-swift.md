---
layout: post
title: "Applying Contains Functions in Swift"
description: " "
date: 2023-09-29
tags: [ContainsFunction]
comments: true
share: true
---

Swift provides several useful functions for working with collections, such as arrays and sets. One of these functions is `contains`, which allows you to check if a collection contains a specific element. In this blog post, we'll explore how to use the `contains` function in Swift.

## The `contains` Function

The `contains` function in Swift is used to check if a collection contains a given element. It returns `true` if the element is found in the collection, and `false` otherwise. The function has two variations:

1. For arrays: 

```swift
let array = [1, 2, 3, 4, 5]
let containsElement = array.contains(3)
```

2. For sets:

```swift
let set: Set = [1, 2, 3, 4, 5]
let containsElement = set.contains(3)
```

In both cases, the `containsElement` variable will be `true` if the element is found in the collection, and `false` otherwise.

## Using `contains` with Strings

The `contains` function can also be used with strings. Let's say we have an array of names, and we want to check if a particular name is present in the array:

```swift
let names = ["Alice", "Bob", "Charlie", "Dave"]
let containsName = names.contains("Charlie")
```

In this case, `containsName` will be `true` because the name "Charlie" is present in the `names` array.

## Case Sensitivity

By default, the `contains` function in Swift performs a case-sensitive search. This means that if you're searching for a string, the function will only consider a match if the case is identical.

To perform a case-insensitive search, you can use the `contains(where:)` method along with the `range(of:options:)` method:

```swift
let names = ["Alice", "Bob", "Charlie", "Dave"]
let containsName = names.contains(where: { $0.range(of: "charlie", options: .caseInsensitive) != nil })
```

By using the `range(of:options:)` method with the `.caseInsensitive` option, we can perform a case-insensitive search and the `containsName` variable will be `true` if the name "Charlie" (ignoring case) is present in the array.

## Conclusion

The `contains` function in Swift is a handy tool for checking if a collection contains a specific element. Whether you're working with arrays, sets, or strings, the `contains` function can help you easily determine if an element exists in your collection.

#Swift #ContainsFunction