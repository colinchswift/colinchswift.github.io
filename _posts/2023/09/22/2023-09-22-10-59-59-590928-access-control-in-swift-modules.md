---
layout: post
title: "Access Control in Swift Modules"
description: " "
date: 2023-09-22
tags: [swift, accesscontrol]
comments: true
share: true
---

In Swift, access control is an important feature that allows you to control the visibility and availability of classes, structs, properties, methods, and other entities within your codebase. This helps in encapsulating implementation details and maintaining code integrity.

### Default Access Levels

By default, a Swift entity has internal access within its module. This means that it can be accessed by any code within the same module, but not from outside the module. This is a good level of access for most scenarios.

### Public Access Level

The `public` access level is the highest level of access control in Swift. When a class, struct, property, or method is marked as `public`, it can be accessed from any source file within the same module or from any other module that imports the module where the entity is defined.

```swift
public class ExampleClass {
    public var publicProperty: Int = 10

    public init() {}
    
    public func publicMethod() {}
}
```

### Internal Access Level

The `internal` access level is the default level and allows entities to be accessed within the same module. You don't need to explicitly specify the internal access level, as it is the default access level for entities that don't have an access control modifier.

```swift
class InternalClass {
    var internalProperty: Int = 20

    init() {}

    func internalMethod() {}
}
```

### Private Access Level

The `private` access level restricts the visibility of an entity to the enclosing declaration or to extensions of that declaration within the same source file.

```swift
class PrivateClass {
    private var privateProperty: Int = 30

    private init() {}

    private func privateMethod() {}
}
```

### Access Control Modifiers for Extensions

When extending a class, struct, or enum, you can use access control modifiers to define the access level of the extensions. This allows you to add additional methods or properties with different access levels than the original entity.

```swift
extension ExampleClass {
    public func extensionMethod() {}

    internal var extensionProperty: String {
        return "Extension Property"
    }
}
```

### Summary

In Swift, access control helps in writing modular and maintainable code. By carefully defining the access levels for your entities, you can control the visibility and availability of your code across modules. Remember to use the appropriate access level to protect sensitive parts of your code and to maintain a clean and encapsulated codebase.

#swift #accesscontrol