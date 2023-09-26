---
layout: post
title: "File-private Access Level in Swift"
description: " "
date: 2023-09-22
tags: [AccessLevels]
comments: true
share: true
---

In Swift, access levels define the scope and visibility of entities like variables, functions, and types. One of the available access levels is `file-private`, which restricts access to within the file that defines the entity.

## Syntax

To declare an entity with `file-private` access, simply add the `fileprivate` keyword before its declaration. Here's an example:

```swift
fileprivate var secretCode = "123456"

fileprivate func unlockSecret() {
    // Code to unlock secret
}

fileprivate struct SecretStruct {
    // Code for the secret struct
}
```

## Characteristics and Usage

File-private access has the following characteristics:

1. **Scope**: Entities marked with `file-private` access are accessible only within the file that contains their declaration.

2. **Module Exclusion**: Unlike other access levels, `file-private` entities are not accessible from other files within the same module. This ensures that the entity is truly restricted to the file where it is declared.

3. **Nested Accessibility**: Entities declared within a `file-private` entity (e.g., a function within a `file-private` struct) have access to the `file-private` entity. However, these nested entities are still restricted to the file.

File-private access is useful in scenarios where you want to encapsulate implementation details and enforce stronger encapsulation within a file. By limiting access to only the file where the entity is defined, you reduce the risk of unwanted modifications or dependencies from other parts of your codebase.

## Conclusion

The `file-private` access level in Swift provides a way to restrict the scope of entities to the file in which they are defined. By using `file-private`, you can enforce encapsulation and better organize your code. It helps avoid unwanted modifications and dependencies from other parts of your program.

#Swift #AccessLevels