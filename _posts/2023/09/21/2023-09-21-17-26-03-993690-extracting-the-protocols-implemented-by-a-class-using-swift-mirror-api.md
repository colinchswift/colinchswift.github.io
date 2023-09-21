---
layout: post
title: "Extracting the protocols implemented by a class using Swift Mirror API"
description: " "
date: 2023-09-21
tags: [SwiftTips, RuntimeReflection]
comments: true
share: true
---

One powerful feature of Swift is the ability to inspect objects and classes at runtime using the Mirror API. This API provides us with a way to examine the structure, properties, and other characteristics of Swift objects.

In this blog post, we will focus on how to use the Mirror API to extract the protocols implemented by a class. This can be useful in cases where you want to dynamically determine the protocols conforming to a class, without static knowledge of them.

To start, let's define a class that conforms to one or more protocols:

```swift
class MyClass: Protocol1, Protocol2, Protocol3 {
    ...
}
```

To extract the protocols implemented by an instance of `MyClass`, we need to create a mirror for that instance and iterate through its children. Each child represents a property or associated value of the instance. In this case, we are interested in the `children` that represent the protocols implemented by the class.

Here's an example of how to extract the protocols implemented by a class using the Mirror API:

```swift
func extractProtocols(from instance: Any) -> [String] {
    let mirror = Mirror(reflecting: instance)
    var protocols: [String] = []

    for child in mirror.children {
        if let typeName = child.value as? Any.Type {
            let protocolName = String(describing: typeName)
            if protocolName.starts(with: "Protocol") {
                protocols.append(protocolName)
            }
        }
    }
    
    return protocols
}
```

In the code above, we create a function `extractProtocols(from:)` that takes an instance of any class or struct as a parameter and returns an array of protocol names.

We start by creating a mirror using `Mirror(reflecting: instance)` to reflect the instance. Then, we iterate through each child of the mirror and check if the child's value is of type `Any.Type`, which represents a protocol. We extract the name of the protocol using `String(describing: typeName)` and check if it starts with the desired prefix, such as "Protocol". If it matches, we append it to the protocols array.

Now, we can use this function to extract the protocols implemented by a class:

```swift
let myObject = MyClass()
let implementedProtocols = extractProtocols(from: myObject)
print(implementedProtocols)
```

This will print an array of protocol names implemented by the `MyClass` instance.

In conclusion, by using the Swift Mirror API, we can dynamically extract the protocols implemented by a class. This provides us with powerful introspection capabilities and allows us to work with protocols at runtime. #SwiftTips #RuntimeReflection