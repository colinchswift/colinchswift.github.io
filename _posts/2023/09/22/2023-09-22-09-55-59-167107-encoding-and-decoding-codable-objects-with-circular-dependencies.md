---
layout: post
title: "Encoding and decoding Codable objects with circular dependencies"
description: " "
date: 2023-09-22
tags: [Coding, Swift]
comments: true
share: true
---

When working with Codable objects in Swift, it is common to encounter scenarios where objects have circular dependencies, meaning that two or more objects reference each other. This can create challenges when encoding and decoding these objects since the default Codable implementation provided by Swift does not handle circular dependencies out of the box. In this article, we will explore a solution to encode and decode Codable objects with circular dependencies.

## Understanding the Problem

Consider the following scenario: we have two types, `Person` and `Family`, where a `Person` can have a reference to a `Family` and a `Family` can have an array of `Person` objects. This creates a circular dependency, as a `Person` is part of a `Family`, while a `Family` contains a collection of `Person` objects.

```swift
struct Person: Codable {
    let name: String
    weak var family: Family?
}

struct Family: Codable {
    let members: [Person]
}
```

## Encoder and Decoder Implementation

To handle the circular dependency, we need to customize the encoding and decoding process. This can be done by overriding the `encode(to:)` and `init(from:)` methods.

```swift
struct Person: Codable {
    let name: String
    weak var family: Family?

    enum CodingKeys: String, CodingKey {
        case name
    }

    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(name, forKey: .name)
    }

    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        name = try container.decode(String.self, forKey: .name)
    }
}

struct Family: Codable {
    let members: [Person]

    enum CodingKeys: String, CodingKey {
        case members
    }

    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(members, forKey: .members)
    }

    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        members = try container.decode([Person].self, forKey: .members)
    }
}
```

## Breaking the Circular Dependency

To break the circular dependency, we need to make one of the properties in either `Person` or `Family` optional. Let's make the `family` property in `Person` optional to resolve the circular dependency.

```swift
struct Person: Codable {
    let name: String
    weak var family: Family?
}
```

## Recursive Encoding and Decoding

To properly encode and decode the circular dependency, we need to add additional logic to our encoding and decoding methods. We can utilize the `userInfo` parameter in the `Encoder` and `Decoder` to keep track of the objects that have already been encoded or decoded.

```swift
struct Person: Codable {
    let name: String
    weak var family: Family?

    enum CodingKeys: String, CodingKey {
        case name
    }

    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(name, forKey: .name)

        var userInfo = encoder.userInfo
        var encodedObjects = userInfo[.encodedObjects] as? Set<ObjectIdentifier> ?? Set<ObjectIdentifier>()
        encodedObjects.insert(ObjectIdentifier(self))
        userInfo[.encodedObjects] = encodedObjects

        try family?.encode(to: encoder)
    }

    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        name = try container.decode(String.self, forKey: .name)

        let userInfo = decoder.userInfo
        var decodedObjects = userInfo[.decodedObjects] as? Set<ObjectIdentifier> ?? Set<ObjectIdentifier>()
        decodedObjects.insert(ObjectIdentifier(self))
        userInfo[.decodedObjects] = decodedObjects

        family = try Family(from: decoder)
    }
}
```

## Handling Circular Dependencies

To handle circular dependencies, we need to add additional logic in the encoding and decoding methods. We can keep track of the objects that have already been encoded or decoded by storing them in the `userInfo`.

```swift
struct Family: Codable {
    let members: [Person]

    enum CodingKeys: String, CodingKey {
        case members
    }

    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)

        var encodedObjects = encoder.userInfo[.encodedObjects] as? Set<ObjectIdentifier> ?? Set<ObjectIdentifier>()
        let shouldEncode = encodedObjects.insert(ObjectIdentifier(self)).inserted

        if shouldEncode {
            try container.encode(members, forKey: .members)
        }
    }

    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)

        var decodedObjects = decoder.userInfo[.decodedObjects] as? Set<ObjectIdentifier> ?? Set<ObjectIdentifier>()
        let shouldDecode = decodedObjects.insert(ObjectIdentifier(self)).inserted

        if shouldDecode {
            members = try container.decode([Person].self, forKey: .members)
        } else {
            members = []
        }
    }
}
```

## Conclusion

Handling circular dependencies when encoding and decoding Codable objects requires custom implementation of the encoding and decoding methods. By breaking the circular reference and keeping track of the objects already processed using the `userInfo`, we can successfully encode and decode objects with circular dependencies in Swift.

#Coding #Swift #CircularDependencies