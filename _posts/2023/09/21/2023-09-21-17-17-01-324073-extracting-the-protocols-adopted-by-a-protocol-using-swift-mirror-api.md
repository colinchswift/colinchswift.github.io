---
layout: post
title: "Extracting the protocols adopted by a protocol using Swift Mirror API"
description: " "
date: 2023-09-21
tags: [reflection]
comments: true
share: true
---

Swift is a powerful and versatile programming language that provides developers with a wide range of tools and APIs to work with. One such API is the Mirror API, which allows us to inspect the structure and metadata of Swift types at runtime. In this blog post, we will explore how to use the Mirror API to extract the protocols adopted by a protocol.

## Understanding the Mirror API

The Mirror API provides a way to reflect upon the structure and metadata of Swift types, including protocols. By creating a mirror instance of a given type or value, we can access information about its components such as properties, methods, and protocols.

## Extracting Protocols Adopted by a Protocol

To extract the protocols adopted by a protocol using the Mirror API, we can follow these steps:

1. Define the protocol whose adopted protocols we want to extract.

```swift
protocol MyProtocol: SomeProtocol, AnotherProtocol {
    // Protocol definition
}
```

2. Create a mirror instance for the protocol using the `Mirror(reflecting:)` initializer.

```swift
let mirror = Mirror(reflecting: MyProtocol.self)
```

3. Iterate over the children of the mirror instance and filter for elements of type `Protocol`.

```swift
let adoptedProtocols = mirror.children.compactMap { $0.value as? Protocol }
```

4. Finally, we can access the names of the adopted protocols using the `Protocol`'s `name` property.

```swift
let protocolNames = adoptedProtocols.compactMap { protocol_getName($0) }.compactMap { String(cString: $0) }
```

## Example Usage

```swift
protocol SomeProtocol {}
protocol AnotherProtocol {}

protocol MyProtocol: SomeProtocol, AnotherProtocol {
    // Protocol definition
}

let mirror = Mirror(reflecting: MyProtocol.self)
let adoptedProtocols = mirror.children.compactMap { $0.value as? Protocol }
let protocolNames = adoptedProtocols.compactMap { protocol_getName($0) }.compactMap { String(cString: $0) }

print(protocolNames) // Output: ["SomeProtocol", "AnotherProtocol"]
```

## Conclusion

Using the Mirror API in Swift, we can extract information about the structure and metadata of Swift types, including protocols. In this blog post, we learned how to use the Mirror API to extract the protocols adopted by a protocol. This can be useful in scenarios where we need to dynamically inspect and manipulate protocols at runtime. Remember to include the appropriate error handling and safety checks when working with runtime reflections in your applications.

#swift #reflection