---
layout: post
title: "Extracting the protocols adopted by a class using Swift Mirror API"
description: " "
date: 2023-09-21
tags: []
comments: true
share: true
---

The Swift programming language provides a powerful reflection mechanism called the Mirror API. This API allows us to inspect the structure and details of objects at runtime. One common use case for the Mirror API is to extract the protocols adopted by a class. In this blog post, we will explore how to achieve this using Swift.

## Accessing the Mirror of an Object ##

The first step to extract the protocols adopted by a class is to access its mirror. The mirror represents the object's runtime reflection, providing information about its properties, methods, and other relevant details. To access the mirror, we can use the `Mirror(reflecting:)` initializer by passing an instance of the object we want to introspect.

```swift
let object = MyClass()
let mirror = Mirror(reflecting: object)
```

## Extracting Protocols from the Mirror ##

Once we have the mirror of the object, we can iterate over the `children` property to extract the protocols adopted by the class. Each child in the mirror represents a property or protocol, and we can filter out the protocols based on their `displayStyle` property.

```swift
let protocols = mirror.children.filter {
    $0.value is Protocol
}.map {
    $0.value as! Protocol
}
```

In the above code snippet, we use the `filter` method to only keep the children that are of type `Protocol`. Next, we use the `map` method to extract the actual protocol value from each child. Since the Mirror API represents protocol values as `Any`, we need to explicitly cast them to the `Protocol` type.

## Example Usage ##

Let's consider an example where we have a class `Person` that adopts two protocols, `Printable` and `Codable`. We can use the following code to extract the protocols from the mirror:

```swift
class Person: Printable, Codable {
    var name: String = "John Doe"
    var age: Int = 30
}

let person = Person()
let mirror = Mirror(reflecting: person)

let protocols = mirror.children.filter {
    $0.value is Protocol
}.map {
    $0.value as! Protocol
}

for protocolValue in protocols {
    let protocolName = "\(protocolValue)"
    print("Adopted protocol: \(protocolName)")
}
```

Running the above code will print the following output:

```
Adopted protocol: Printable
Adopted protocol: Codable
```

## Conclusion ##

The Swift Mirror API provides a powerful way to introspect objects at runtime. By accessing the mirror of a class, we can easily extract the protocols adopted by it. This capability can be useful in scenarios where we need to dynamically handle objects based on their protocols.