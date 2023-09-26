---
layout: post
title: "Encoding and decoding data models with metadata and annotations using Codable"
description: " "
date: 2023-09-22
tags: [EncodingDecoding]
comments: true
share: true
---

In modern software development, handling data models is an integral part of many applications. Whether it's parsing JSON responses from an API, reading and writing data to a database, or serializing and deserializing data for various purposes, encoding and decoding data models effectively is key.

One approach to achieve this in Swift is by using the `Codable` protocol. Introduced in Swift 4, Codable provides a convenient and type-safe way to encode and decode data models to and from different formats such as JSON and property lists.

To enhance the encoding and decoding process, we can leverage metadata and annotations to add additional information to the data models. This can be useful for mapping different property names, handling custom serialization logic, or ignoring certain properties during encoding or decoding.

## Metadata

Metadata provides additional information about a type or its properties. In the context of encoding and decoding data models, it can be used to define custom key mappings and ignore certain properties.

### Key Mapping

In some cases, the property names in the data model might not match the keys in the encoded format. To handle this, we can use the `CodingKeys` enum and specify the mapping between property names and keys.

```swift
struct Person: Codable {
    let firstName: String
    let lastName: String
    let age: Int
    
    enum CodingKeys: String, CodingKey {
        case firstName = "first_name"
        case lastName = "last_name"
        case age
    }
}
```
In the above example, the `CodingKeys` enum maps the `firstName` property to the key "first_name" and `lastName` property to "last_name". The `age` property does not need mapping as the key matches its name.

### Property Ignoring

Sometimes, we may want to exclude specific properties from being encoded or decoded. We can achieve this by marking properties as `@transient` using property wrappers, or by implementing a `CodingKeys` enum and omitting the properties we want to ignore.

```swift
struct Person: Codable {
    let firstName: String
    let lastName: String
    
    @transient
    let age: Int
}
```

In the above example, the `age` property is marked as `@transient`, indicating that it should be ignored during encoding or decoding.

## Annotations

Annotations provide additional instructions or metadata to certain properties within a data model. They allow us to define custom serialization logic and handle complex scenarios during encoding and decoding.

### Custom Serialization

Suppose we have a `Date` property that needs to be formatted in a specific way during encoding and decoding. We can achieve this by implementing custom coding methods in the data model using the `Codable` protocol.

```swift
struct Person: Codable {
    let name: String
    let dateOfBirth: Date
    
    enum CodingKeys: String, CodingKey {
        case name
        case dateOfBirth = "dob"
    }
    
    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(name, forKey: .name)
        let dateFormatter = DateFormatter()
        dateFormatter.dateFormat = "yyyy-MM-dd"
        let formattedDate = dateFormatter.string(from: dateOfBirth)
        try container.encode(formattedDate, forKey: .dateOfBirth)
    }
    
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        name = try container.decode(String.self, forKey: .name)
        let dateFormatter = DateFormatter()
        dateFormatter.dateFormat = "yyyy-MM-dd"
        let formattedDate = try container.decode(String.self, forKey: .dateOfBirth)
        dateOfBirth = dateFormatter.date(from: formattedDate) ?? Date()
    }
}
```

In the above example, the `dateOfBirth` property is mapped to the key "dob", and the `encode(to:)` and `init(from:)` methods are overridden to implement custom serialization logic. This allows us to control how the `Date` property is encoded and decoded.

## Conclusion

Using the Codable protocol in Swift provides a powerful way to encode and decode data models. By leveraging metadata and annotations, we can further enhance the encoding and decoding process. Key mapping allows us to handle property names that don't match the encoded format, while property ignoring allows us to exclude specific properties. Custom serialization logic can be implemented through annotations, providing flexibility in handling complex scenarios.

#Swift #EncodingDecoding