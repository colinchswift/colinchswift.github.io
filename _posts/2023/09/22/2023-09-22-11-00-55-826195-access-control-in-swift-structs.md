---
layout: post
title: "Access Control in Swift Structs"
description: " "
date: 2023-09-22
tags: [AccessControl]
comments: true
share: true
---

When working with structs in Swift, it is important to understand access control and how it affects the visibility and usage of properties and methods within the struct. Access control allows you to specify the level of access that different parts of your code have to the struct's members. In this blog post, we will explore the different access control levels in Swift and how they can be applied to structs.

## Public Access

The `public` access level allows the members of the struct to be accessed from any source file within the module as well as from outside the module. This is the highest level of access and should be used for properties and methods that need to be publicly accessible. To mark a property or method as `public`, you can use the `public` keyword before its declaration.

```swift
public struct MyStruct {
    public var publicProperty: Int = 10
    
    public func publicMethod() {
        // Method implementation
    }
}
```

## Internal Access

The `internal` access level is the default access level in Swift if no access modifier is specified. It allows the members of the struct to be accessed within any source file within the module but not from outside the module. This means that properties and methods marked as `internal` can be used by other code in the same module but are not visible to code outside the module.

```swift
struct MyStruct {
    internal var internalProperty: Int = 20
    
    internal func internalMethod() {
        // Method implementation
    }
}
```

## Private Access

The `private` access level restricts the visibility of the members of the struct to the enclosing declaration. This means that properties and methods marked as `private` can only be accessed from within the same struct. This level of access is useful for encapsulating implementation details and preventing external code from accessing or modifying internal state.

```swift
struct MyStruct {
    private var privateProperty: Int = 30
    
    private func privateMethod() {
        // Method implementation
    }
}
```

It is important to note that the access control levels are hierarchical, meaning that a higher level of access (e.g., `public`) includes all the lower levels of access (e.g., `internal` and `private`). It is also possible to assign access levels to individual members of the struct, allowing for fine-grained control over the visibility of properties and methods.

By understanding and utilizing access control in your Swift structs, you can ensure that your code is organized, secure, and only exposes what is necessary to the outside world.

#Swift #AccessControl