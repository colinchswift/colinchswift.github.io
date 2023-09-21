---
layout: post
title: "Extracting the protocols adopted by a struct using Swift Mirror API"
description: " "
date: 2023-09-21
tags: [swift, mirrorapi]
comments: true
share: true
---

When working with Swift, sometimes you may need to analyze the structure of a type at runtime. The Swift `Mirror` API allows you to do just that by providing a way to introspect and inspect the structure of a type, including its properties, methods, and protocols.

In this article, we will explore how to use the Swift `Mirror` API to extract the protocols adopted by a struct. This can be useful in cases where you want to dynamically determine if a struct conforms to a specific protocol or extract information about the protocols it adopts.

## Getting Started

To begin, let's define a simple struct that adopts a few protocols:

```swift
struct MyStruct: Codable, Equatable, Hashable {
    var name: String
    var age: Int
}
```

## Using the Mirror API

The first step is to create a `Mirror` instance for the struct using the `Mirror(reflecting:)` initializer:

```swift
let mirror = Mirror(reflecting: MyStruct.self)
```

Next, we can iterate over the `mirror.children` collection to inspect the properties of the struct:

```swift
for child in mirror.children {
    print(child.label ?? "")
}
```

This will print out the names of the properties of the `MyStruct`:

```
name
age
```

To extract the protocols adopted by the struct, we can use the `mirror.superclassMirror()` method to get the `Mirror` instance for the struct's superclass, and then access the `mirror.superclassMirror?.subjectType` property to get the superclass type.

```swift
let superclassMirror = mirror.superclassMirror()
let superclassType = superclassMirror?.subjectType
```

Now, we can check if the superclass type conforms to any protocol by using the `superclassType.conforms(to:)` method. This method takes a protocol type as an argument and returns a boolean value indicating if the superclass type conforms to that protocol.

```swift
if let superclassType = superclassType {
    let isCodable = superclassType.conforms(to: Codable.self)
    let isEquatable = superclassType.conforms(to: Equatable.self)
    let isHashable = superclassType.conforms(to: Hashable.self)

    if isCodable {
        print("MyStruct adopts the Codable protocol")
    }
    if isEquatable {
        print("MyStruct adopts the Equatable protocol")
    }
    if isHashable {
        print("MyStruct adopts the Hashable protocol")
    }
}
```

## Conclusion

Using the Swift `Mirror` API, we can extract the protocols adopted by a struct at runtime. This allows us to dynamically analyze the structure of the type and determine if it conforms to specific protocols. This can be particularly helpful in cases where you need to perform operations specific to certain protocols.

Remember to import the `Swift` module to access the `Mirror` API, and feel free to explore other capabilities of the `Mirror` API to further analyze and inspect types at runtime.

#swift #mirrorapi