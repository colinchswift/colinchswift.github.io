---
layout: post
title: "Internal Access Level in Swift"
description: " "
date: 2023-09-22
tags: [access]
comments: true
share: true
---

In Swift, access levels are used to define the visibility and availability of classes, structs, properties, functions, and other entities. One of the access levels available in Swift is the **internal** access level.

## What is internal access level?

The **internal** access level is the default access level in Swift if no access level modifier is explicitly specified. Entities with internal access level are accessible within any source file that belongs to the same module. 

A **module** in Swift refers to a single unit of code distribution, such as a framework or an application. It is a way to encapsulate related code and provide a clear boundary for access control.

## How to declare internal access level?

To declare an entity with internal access level, you simply omit any access level modifier. For example, consider the following code snippet:

```swift
// MyModule.swift

struct MyStruct {
  internal var myProperty: Int = 10

  internal func myFunction() {
    // Function implementation
  }
}
```

In the above example, the `MyStruct` struct and its properties and functions are declared with internal access level. This means they can be accessed from anywhere within the same module, but not from external modules.

## Advantages of internal access level

Using internal access level in Swift can provide several advantages:

1. **Encapsulation**: Internal access level allows you to define the boundaries of your module and encapsulate related code within it. This helps in organizing your codebase and makes it easier to understand and maintain.

2. **Protecting implementation details**: By using internal access level, you can hide the implementation details of your module from external modules. This provides a clear separation between the public interface and the implementation details, allowing you to change the implementation without affecting the users of your module.

3. **Unit testing**: Internal access level allows unit tests to access the entities of your module, enabling thorough testing of your code. Since internal entities are accessible within the same module, you can write test cases that cover different scenarios and ensure the correctness of your implementation.

## Conclusion

Internal access level in Swift provides a good balance between encapsulation and accessibility. It allows entities to be accessible within the same module, providing control over the visibility and availability of your code. By using internal access level effectively, you can design well-structured modules and maintain a clean and reliable codebase.

#swift #access-level