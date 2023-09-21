---
layout: post
title: "Creating a custom mirror for a struct using Swift Mirror API"
description: " "
date: 2023-09-21
tags: [Swift, MirrorAPI]
comments: true
share: true
---

In Swift, the Mirror API allows you to inspect the properties and children of a type. It provides valuable information about the structure of a type, including its properties, their types, and other metadata.

By default, the Mirror API automatically generates a mirror for any type that does not provide a custom implementation. However, there may be cases where you want to customize the mirror for a struct to include specific properties or exclude certain ones.

Here's an example of how you can create a custom mirror for a struct using the Swift Mirror API.

```swift
struct Person {
    let name: String
    let age: Int
}

extension Person: CustomReflectable {
    var customMirror: Mirror {
        let children = [
            "name": name,
            "age": age
        ]
        
        return Mirror(self, children: children, displayStyle: .struct)
    }
}

let person = Person(name: "John Doe", age: 30)
let mirror = Mirror(reflecting: person)

for case let (label?, value) in mirror.children {
    print("\(label): \(value)")
}
```

In the above code, we define a `Person` struct with `name` and `age` properties. We then extend the struct to conform to the `CustomReflectable` protocol, which requires implementing the `customMirror` property.

Inside the `customMirror` implementation, we create a dictionary of children where each key-value pair represents the label and value of a property we want to include in the mirror. In this case, we include `name` and `age`.

Finally, we create an instance of `Person` and obtain its mirror using `Mirror(reflecting:)` and iterate over its children to print the label and value of each property.

By providing a custom mirror implementation, we have control over the properties and metadata that are reflected, allowing us to customize the behavior according to our needs.

#Swift #MirrorAPI