---
layout: post
title: "Encoding and decoding Swift objects with circular references using Codable and weak/unowned references"
description: " "
date: 2023-09-22
tags: [Swift, CircularReferences]
comments: true
share: true
---

Circular references occur when two or more objects reference each other in a way that forms a loop. While this can be convenient for modeling certain relationships in an application, it poses a challenge when it comes to encoding and decoding objects using Swift's Codable protocol.

When encoding an object with a circular reference using Codable, the default behavior would attempt to serialize the entire object graph, resulting in an infinite loop. Similarly, when decoding an object with a circular reference, the default behavior would lead to duplicate objects, since the decoder doesn't recognize that the objects are already present.

To overcome this obstacle, we can use **weak** or **unowned** references within the circular relationship and implement custom encoding and decoding logic. Let's explore an example to demonstrate this approach.

```swift
class Person: Codable {
    var name: String
    weak var bestFriend: Person? // Using weak reference
    
    init(name: String, bestFriend: Person? = nil) {
        self.name = name
        self.bestFriend = bestFriend
    }
}
```

In this example, we have a `Person` class that contains a `name` property and a `bestFriend` property, which is a weak reference to another `Person` object.

To encode and decode `Person` objects, we need to implement the `encode(to:)` and `init(from:)` methods of the `Codable` protocol respectively.

```swift
extension Person {
    enum CodingKeys: String, CodingKey {
        case name
    }
    
    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(name, forKey: .name)
    }
    
    convenience required init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        let name = try container.decode(String.self, forKey: .name)
        
        self.init(name: name)
    }
}
```

In this implementation, we only encode and decode the `name` property of the `Person` object. **Note**: We intentionally exclude the `bestFriend` property to avoid the circular reference issue.

Now, let's handle the circular reference by encoding the `bestFriend` property manually using a custom coding key.

```swift
extension Person {
    enum CodingKeys: String, CodingKey {
        case name
        case bestFriend
    }
    
    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(name, forKey: .name)

        if let bestFriend = bestFriend {
            // Encode reference to bestFriend using custom coding key
            try container.encode(bestFriend, forKey: .bestFriend)
        }
    }
    
    convenience required init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        let name = try container.decode(String.self, forKey: .name)
        
        self.init(name: name)
        
        // Decode reference to bestFriend using custom coding key
        self.bestFriend = try container.decodeIfPresent(Person.self, forKey: .bestFriend)
    }
}
```

In this updated implementation, we encode the `bestFriend` property manually using a custom coding key, `bestFriend`. The `encode(to:)` method checks if `bestFriend` is not-nil, and if so, encodes a reference to it. Similarly, in the `init(from:)` method, we decode the `bestFriend` property using the custom coding key.

Using this approach, circular references are handled correctly during the encoding and decoding process, without causing infinite loops or duplicate object instances.

Remember to handle circular references with care, and choose the appropriate reference type (`weak` or `unowned`) based on your application's requirements. With the use of Codable and weak/unowned references, you can effectively encode and decode Swift objects with circular references.

#Swift #CircularReferences