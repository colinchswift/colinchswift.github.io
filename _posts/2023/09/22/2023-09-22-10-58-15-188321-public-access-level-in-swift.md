---
layout: post
title: "Public Access Level in Swift"
description: " "
date: 2023-09-22
tags: [SwiftProgramming, AccessLevels]
comments: true
share: true
---

When writing code in Swift, you have different access levels that you can apply to your variables, functions, classes, and other entities. One of these access levels is `public`, which allows those entities to be accessed from any source file or module that imports the module they are defined in.

By using the `public` access level, you are making your code accessible to other developers and modules outside your own module. This is particularly useful when you want to create reusable frameworks or libraries that can be used in various projects.

To define an entity with a `public` access level in Swift, simply add the `public` keyword before the entity's declaration. For example:

```swift
public class MyClass {
    public var myProperty: String = "Hello, public!"
    
    public init() { }
    
    public func myPublicMethod() {
        print("This method is accessible from any source file or module.")
    }
}
```

In the above example, the `MyClass` class, its `myProperty` property, `init()` initializer, and `myPublicMethod()` method are all marked as `public`. This means that they can be accessed from any other module that imports the module where `MyClass` is defined.

It's important to note that `public` entities have the highest level of accessibility in Swift. However, they are still subject to other restrictions such as module boundaries and inheritance, as defined by the language.

To summarize, using the `public` access level in Swift allows you to make your code accessible to other modules and source files. This enables you to create reusable and sharable frameworks or libraries. Just remember to use the `public` keyword to mark the entities you want to expose to the world!

#SwiftProgramming #AccessLevels