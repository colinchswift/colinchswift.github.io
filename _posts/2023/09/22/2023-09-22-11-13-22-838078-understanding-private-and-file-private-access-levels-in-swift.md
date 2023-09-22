---
layout: post
title: "Understanding Private and File-private Access Levels in Swift"
description: " "
date: 2023-09-22
tags: [SwiftProgramming, AccessLevels]
comments: true
share: true
---

When programming in Swift, it's important to understand access levels to control the visibility and interaction between different parts of your code. Swift provides several access levels, including `private` and `file-private`, that help you enforce encapsulation and protect the internal implementation of your code.

## Private Access Level

The `private` access level restricts the usage of an entity to the scope in which it is defined. This means that the entity can only be accessed within the same source file where it is declared. It is not available to any other files, even if they are part of the same module.

To declare a `private` entity, use the `private` keyword before its declaration:

```swift
private var secretValue = "hidden"
private func doSomething() { }
```

In the example above, the `secretValue` variable and the `doSomething()` function are both `private` and can only be accessed within the source file where they are declared. Any attempt to access them from another source file will result in a compilation error.

## File-private Access Level

The `file-private` access level restricts the usage of an entity to the source file in which it is defined. Unlike `private`, `file-private` allows access to the entity from any code within the same source file, regardless of their relationship or belonging to the same module.

To declare a `file-private` entity, use the `fileprivate` keyword before its declaration:

```swift
fileprivate var internalValue = 42
fileprivate struct Employee { }
```

In the code snippet above, the `internalValue` variable and the `Employee` struct are both `file-private`. They can be accessed from any code within the same source file, but not from any other source file outside of it.

## Conclusion

By using the `private` and `file-private` access levels in Swift, you can control the visibility and access of your code entities, ensuring encapsulation and safeguarding the internal implementation details. Understanding and using these access levels appropriately will help you write more modular and maintainable Swift code.

#SwiftProgramming #AccessLevels