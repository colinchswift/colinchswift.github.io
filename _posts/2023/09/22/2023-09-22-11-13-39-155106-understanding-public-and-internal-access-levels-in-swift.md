---
layout: post
title: "Understanding Public and Internal Access Levels in Swift"
description: " "
date: 2023-09-22
tags: [swift, access]
comments: true
share: true
---

Swift is a powerful and modern programming language that provides different access levels to control the visibility and availability of code entities like classes, structs, functions, and more. Two common access levels in Swift are **public** and **internal**. In this article, we will explore the differences between these access levels and when to use them.

## Public Access Level

The **public** access level allows entities to be accessed from any source file or module, including external modules that import the current module. When a class, struct, function, or property is marked as public, it can be used and accessed by other modules, making it ideal for creating reusable code.

To declare an entity with **public** access level, use the keyword `public` before the entity's declaration:

```swift
public class MyPublicClass {
    public var publicProperty: Int = 10

    public func publicMethod() {
        // Public method implementation
    }
}
```

## Internal Access Level

In contrast, the **internal** access level limits the scope of an entity's visibility to the module in which it is defined. Entities declared with **internal** access level can be accessed from any source file within the same module but are not available outside of it.

By default, if you don't specify an access level explicitly, entities are given internal access level. However, it's a good practice to explicitly declare the access level to avoid any confusion:

```swift
internal struct MyInternalStruct {
    internal var internalProperty: String = "Internal Property"

    internal func internalMethod() {
        // Internal method implementation
    }
}
```

## Choosing the Right Access Level

When deciding which access level to use, it's important to consider the intended usage and scope of the code entity. Here are some guidelines to help you make the right choice:

- Use **public** access level for entities that need to be accessed from outside the module, such as a public API of a framework or library.
- Use **internal** access level for entities that are only used within the current module and to hide implementation details from other modules.

Remember, access levels promote encapsulation and modular development by controlling the visibility of code entities. By properly setting access levels, you can ensure the right level of abstraction and maintainability of your Swift codebase.

#swift #access-levels