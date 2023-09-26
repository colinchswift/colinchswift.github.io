---
layout: post
title: "Extracting the protocols implemented by a struct using Swift Mirror API"
description: " "
date: 2023-09-21
tags: [SwiftMirrorAPI]
comments: true
share: true
---

In Swift, we can use the `Mirror` API to extract information about the structure and content of a given type. One useful application of this API is to determine the protocols that a struct implements. In this blog post, we will explore how to extract the protocols implemented by a struct using the Swift Mirror API.

## The Mirror API

The `Mirror` is a Swift standard library type that allows us to inspect the properties, methods, and other characteristics of a given instance. This API is particularly useful when working with structs, as it provides a way to dynamically introspect their structure.

## Extracting Protocols

To determine the protocols implemented by a struct, we can use the `MirroredType` property of the `Mirror` instance. This property returns a collection of `Protocol` types that the struct conforms to.

Here's an example code snippet that demonstrates how to extract the protocols implemented by a struct using the Swift Mirror API:

```swift
struct Person: Codable, Equatable {
    var name: String
    var age: Int
}

func getImplementedProtocols(of object: Any) -> [String] {
    let mirror = Mirror(reflecting: object)
    return mirror.subjectType
        .conformingToProtocol
        .map { String(describing: $0) }
}

let person = Person(name: "John", age: 30)
let protocols = getImplementedProtocols(of: person)
print(protocols)
```

In the above code, we have a struct `Person` that implements two protocols: `Codable` and `Equatable`. The `getImplementedProtocols` function accepts any object and uses the `Mirror` API to extract the protocols implemented by the given object. The function returns an array of protocol names as strings.

By calling `getImplementedProtocols` with our `person` object, we will get an array containing the following elements: `["Codable", "Equatable"]`. We can then use this information for further processing or logging purposes.

## Conclusion

Using the Swift Mirror API, we can easily extract the protocols implemented by a struct. This can be useful in various scenarios, such as runtime inspection or validation of protocol conformance. The `Mirror` API provides powerful introspection capabilities that enable us to gather information about the structure and characteristics of our types.

Remember to **#Swift** and **#SwiftMirrorAPI** when sharing this post on social media to help others discover this valuable technique!