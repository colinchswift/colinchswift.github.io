---
layout: post
title: "Encoding and decoding Swift objects with custom serialization rules using Codable"
description: " "
date: 2023-09-22
tags: [SwiftProgramming, Codable]
comments: true
share: true
---

Serialization is the process of converting an object's state into a format that can be stored or transmitted. On the other hand, deserialization is the process of rebuilding the object back from the serialized form. Swift provides a powerful and convenient way to perform serialization and deserialization through the `Codable` protocol.

The `Codable` protocol combines the functionalities of `Encodable` and `Decodable`. By conforming to the `Codable` protocol, you can easily convert Swift objects to and from serialized data such as JSON or property lists.

However, in some cases, you might encounter scenarios where the default serialization and deserialization rules provided by the `Codable` protocol do not meet your requirements. For instance, you might need to encode and decode Swift objects with custom representations or perform additional transformations during serialization and deserialization. In such cases, you can define your own encoding and decoding logic by extending the `Encoding` and `Decoding` protocols.

Here's an example of encoding and decoding Swift objects with custom serialization rules using `Codable`.

## Custom Encoding

To define custom encoding rules, you need to extend the `Encoding` protocol and implement the `encode(to:)` method. This method takes a `Encoder` object as a parameter, which you can use to encode the properties of your object.

Here's an example where we define a custom encoding rule for a `Person` struct:

```swift
struct Person: Codable {
    let name: String
    let age: Int

    enum CodingKeys: String, CodingKey {
        case personName, personAge
    }

    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(name, forKey: .personName)
        try container.encode(age + 10, forKey: .personAge)
    }
}
```

In this example, we use the `CodingKeys` enum to provide custom key mappings for encoding the properties. We also modify the `age` property by adding 10 before encoding it.

## Custom Decoding

To define custom decoding rules, you need to extend the `Decoding` protocol and implement the `init(from:)` initializer. This initializer takes a `Decoder` object as a parameter, which you can use to decode the properties of your object.

Here's an example where we define a custom decoding rule for the `Person` struct:

```swift
struct Person: Codable {
    let name: String
    let age: Int

    enum CodingKeys: String, CodingKey {
        case personName, personAge
    }

    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        name = try container.decode(String.self, forKey: .personName)
        age = try container.decode(Int.self, forKey: .personAge) - 10
    }
}
```

In this example, we use the `CodingKeys` enum to provide custom key mappings for decoding the properties. We also modify the decoded value of the `age` property by subtracting 10.

## Conclusion

By extending the `Encoding` and `Decoding` protocols, you can define custom serialization rules for encoding and decoding Swift objects. This allows you to have more control over the serialization and deserialization processes, enabling you to handle complex serialization scenarios and perform transformations as needed. With Swift's `Codable` protocol, you can achieve efficient and convenient object serialization in your applications.

#SwiftProgramming #Codable