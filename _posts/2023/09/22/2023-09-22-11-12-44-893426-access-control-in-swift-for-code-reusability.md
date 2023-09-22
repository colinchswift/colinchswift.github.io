---
layout: post
title: "Access Control in Swift for Code Reusability"
description: " "
date: 2023-09-22
tags: [Swift, AccessControl]
comments: true
share: true
---

In Swift, access control mechanisms play a crucial role in ensuring code reusability, modularity, and maintainability. By properly managing access levels, developers can control the visibility and availability of various entities within their codebase. In this blog post, we will explore different access control levels provided by Swift and discuss how to leverage them effectively.

## Public Access

The `public` access level is the highest level of access control in Swift. When a class, structure, enumeration, or function is marked as `public`, it can be accessed from any source file within the module where it is defined. Additionally, it can also be accessed from other modules that import the module where it is defined.

```swift
public struct Person {
    public var name: String
    public init(name: String) {
        self.name = name
    }
}
```

In the above example, the `Person` struct and its `name` property are marked as `public`. This means that they are accessible from any source file within the module and can be used by other modules as well.

## Internal Access

The `internal` access level is the default level in Swift if no access level modifier is specified. Entities marked as `internal` are accessible within the module where they are defined, but not outside of it. This level of access is suitable for code components that are only needed internally within a module and do not need to be exposed to the outside world.

```swift
internal struct InternalStruct {
    internal var property: Int
    internal init(property: Int) {
        self.property = property
    }
}
```

The `InternalStruct` and its `property` are marked as `internal`, indicating that they are accessible within the same module but not from outside.

## File-private Access

The `fileprivate` access level restricts the visibility of an entity to the source file where it is defined. This means that other source files within the same module cannot access it. This level of access is useful when you want to encapsulate implementation details within a specific file.

```swift
fileprivate class FilePrivateClass {
    fileprivate var secretValue: String
    fileprivate init(secretValue: String) {
        self.secretValue = secretValue
    }
}
```

The `FilePrivateClass` and its `secretValue` property are marked as `fileprivate`, indicating that they can only be accessed within the same file.

## Private Access

The most restrictive access level in Swift is `private`. Entities marked as `private` are only accessible within the enclosing declaration, such as a class or a structure. They are not accessible from any other context, including extensions of the enclosing declaration. This level of access provides the highest level of encapsulation and is useful for hiding implementation details.

```swift
class ParentClass {
    private var privateProperty: Int
    init(privateProperty: Int) {
        self.privateProperty = privateProperty
    }
    
    private func privateMethod() {
        // Implementation goes here
    }
}

extension ParentClass {
    func publicMethod() {
        privateMethod() // Error: 'privateMethod' is inaccessible due to 'private' protection level
    }
}
```

In the above example, the `privateProperty` and `privateMethod` are marked as `private`. They can only be accessed within the `ParentClass` and are not accessible from any other context, including extensions.

## Summary

By leveraging Swift's access control mechanisms, developers can enforce code reusability, encapsulation, and modularity. Choosing the appropriate access levels for various entities ensures that the codebase remains organized and easy to maintain. Understanding and utilizing access control effectively contributes to the overall quality of the Swift code.

#Swift #AccessControl