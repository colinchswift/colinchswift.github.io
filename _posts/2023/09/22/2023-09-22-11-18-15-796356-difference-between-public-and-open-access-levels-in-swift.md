---
layout: post
title: "Difference between Public and Open Access Levels in Swift"
description: " "
date: 2023-09-22
tags: [AccessLevels]
comments: true
share: true
---

When working with Swift, it's important to understand the different access levels available for classes, structs, properties, and methods. Two commonly used access levels are "public" and "open". In this article, we will explore the differences between these two access levels and how they impact the accessibility of Swift code.

## Public Access Level

The "public" access level in Swift allows entities to be accessed from any source file or module, whether it is within the same module or a different one. This means that any part of your application or even external frameworks can access and use the public entities you define.

### Example:
```swift
public class MyClass {
    public var myProperty: Int
    
    public init(property: Int) {
        self.myProperty = property
    }
    
    public func myMethod() {
        print("This is a public method.")
    }
}
```

In the example above, we have defined a public class `MyClass`. The `myProperty` property and `myMethod()` method are also declared with the public access level. This means that they can be accessed and used by any part of the application or even external frameworks.

## Open Access Level

The "open" access level in Swift provides the highest level of access and is mainly used when defining class members that require subclassing or overriding. Similar to public access, open access allows entities to be accessed from any source file or module.

### Example:
```swift
open class MyOpenClass {
    open var myProperty: Int
    
    public init(property: Int) {
        self.myProperty = property
    }
    
    open func myMethod() {
        print("This is an open method.")
    }
}
```

In this example, we have a class `MyOpenClass` that is declared with the open access level. The `myProperty` property and `myMethod()` method are also declared with the open access level. This means that they can be subclassed, overridden, and accessed by any part of the application or external frameworks.

## Conclusion

To summarize, the main difference between the "public" and "open" access levels in Swift is that "open" allows for subclassing and overriding, while "public" does not. Both access levels allow entities to be accessed from any source file or module. It is important to choose the appropriate access level based on the intended usage and requirements of your Swift code.

#Swift #AccessLevels