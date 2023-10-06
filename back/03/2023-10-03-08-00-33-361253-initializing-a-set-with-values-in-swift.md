---
layout: post
title: "Initializing a set with values in Swift"
description: " "
date: 2023-10-03
tags: [sets]
comments: true
share: true
---

When working with sets in Swift, there are multiple ways to initialize a set and add values to it. In this article, we will explore some of the common methods that you can use to initialize a set with values in Swift.

## Initializing an Empty Set

To start with, let's see how to initialize an empty set in Swift:

```swift
var emptySet = Set<Int>()
```

Here, we are using the `Set` type and explicitly specifying the element type `Int` within the angle brackets `<>`. By default, Swift infers the element type based on the values you add to the set.

## Initializing a Set with Values

You can also initialize a set with pre-defined values using array literals. Here's an example:

```swift
var fruits: Set<String> = ["Apple", "Banana", "Orange"]
```

In the above code, we are initializing a set called `fruits` with three strings: "Apple", "Banana", and "Orange". 

## Adding Values to a Set

Once you have initialized a set, you can add or remove values dynamically. To add values to a set, you can use the `insert(_:)` method:

```swift
fruits.insert("Mango")
```

The above code adds the string "Mango" to the `fruits` set.

## Conclusion

In this article, we learned how to initialize a set with values in Swift. We explored initializing an empty set using `Set<Int>()` and initializing a set with pre-defined values using array literals. We also saw how to add values to a set using the `insert(_:)` method. By using these techniques, you can easily work with sets and add the required values to them.

#swift #sets