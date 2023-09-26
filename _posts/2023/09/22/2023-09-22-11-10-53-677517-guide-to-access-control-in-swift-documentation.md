---
layout: post
title: "Guide to Access Control in Swift Documentation"
description: " "
date: 2023-09-22
tags: [AccessControl]
comments: true
share: true
---

Access control is an important aspect of any programming language as it helps protect the internal implementation details of a class or structure from being accessed or modified by other parts of the code. In Swift, access control is used to define the visibility and scope of various entities like classes, variables, and functions.

## Public Access

By default, all entities in Swift have internal access, which means they are visible within the module in which they are defined. However, if you want to make an entity accessible from outside the module, you can declare it as public.

```swift
public class MyClass {
    public var publicVariable = 10
    
    public func publicMethod() {
        // Public method implementation
    }
}
```

In the example above, the `MyClass` and its members are accessible from outside the module. Other modules can import this module and use the `MyClass` and its public members.

## Internal Access

Internal access is the default access level in Swift. Entities with internal access are accessible within the module in which they are defined, but not outside of it. You don't need to specify the `internal` keyword explicitly, as it is inferred by default.

```swift
class InternalClass {
    var internalVariable = 20
    
    func internalMethod() {
        // Internal method implementation
    }
}
```

In this example, `InternalClass` and its members are accessible within the same module but not outside of it. Other modules cannot access or use `InternalClass`.

## Private Access

Private access restricts the visibility of an entity to the enclosing scope, such as a class or a function. Entities with private access are not accessible from outside the enclosing scope.

```swift
class OuterClass {
    private var privateVariable = 30
    
    private func privateMethod() {
        // Private method implementation
    }
    
    func outerMethod() {
        privateVariable = 40
        privateMethod()
    }
}
```

In this example, `privateVariable` and `privateMethod()` are only accessible within the `OuterClass`. They cannot be accessed from outside the class or even from an instance of the `OuterClass`.

## File-private Access

File-private access restricts the visibility of an entity to the file in which it is defined. This means that the entity is accessible from anywhere within the same Swift source file, but not from any other file.

```swift
fileprivate class FilePrivateClass {
    fileprivate var fileprivateVariable = 50
    
    fileprivate func fileprivateMethod() {
        // File-private method implementation
    }
}
```

In the example above, `FilePrivateClass` and its members are accessible only within the same file in which they are defined.

## Conclusion

Access control in Swift provides a way to control the visibility and scope of entities within your codebase. By carefully setting the access level of your classes, variables, and functions, you can ensure that your code is secure, maintainable, and easy to understand.

#Swift #AccessControl