---
layout: post
title: "Encoding and decoding Swift enums with associated values using Codable"
description: " "
date: 2023-09-22
tags: [Swift, Codable]
comments: true
share: true
---

Enums are a powerful feature in Swift that allows you to define a set of values for a particular type. With the introduction of Codable in Swift 4, encoding and decoding data has become much easier. However, encoding and decoding enums with associated values can be a bit tricky. In this blog post, we will explore how to encode and decode Swift enums with associated values using Codable.

## Encoding Enums with Associated Values

To encode an enum with associated values, we need to make our enum conform to the `Codable` protocol. Let's say we have an enum called `Status` which represents the status of a user:

```swift
enum Status: Codable {
    case active
    case inactive(reason: String)
}
```

To encode this enum with associated values, we need to implement the `encode(to:)` function from the `Encodable` protocol. We can use a `switch` statement to handle each case of the enum. For the `.inactive` case, we can encode the associated value using a key and encode its associated value using a nested container:

```swift
func encode(to encoder: Encoder) throws {
    var container = encoder.container(keyedBy: CodingKeys.self)
    switch self {
        case .active:
            try container.encode("active", forKey: .status)
        case .inactive(let reason):
            try container.encode("inactive", forKey: .status)
            
            var nestedContainer = container.nestedContainer(keyedBy: CodingKeys.self, forKey: .reason)
            try nestedContainer.encode(reason, forKey: .reason)
    }
}
```

In the code above, we use a `CodingKeys` enumeration to define the keys for the encoded values.

## Decoding Enums with Associated Values

To decode an enum with associated values, we need to implement the `init(from:)` initializer from the `Decodable` protocol. We can use a `switch` statement to handle each case of the enum. For the `.inactive` case, we can decode the associated value using a nested container:

```swift
init(from decoder: Decoder) throws {
    let container = try decoder.container(keyedBy: CodingKeys.self)
    let status = try container.decode(String.self, forKey: .status)
    
    switch status {
        case "active":
            self = .active
        case "inactive":
            let nestedContainer = try container.nestedContainer(keyedBy: CodingKeys.self, forKey: .reason)
            let reason = try nestedContainer.decode(String.self, forKey: .reason)
            self = .inactive(reason: reason)
        default:
            throw DecodingError.dataCorruptedError(forKey: .status, in: container, debugDescription: "Invalid status")
    }
}
```

## Conclusion

With the introduction of Codable in Swift, encoding and decoding enums with associated values has become much simpler. By implementing the `Codable` protocol, you can easily encode and decode Swift enums with associated values. This allows you to serialize and deserialize complex data structures with ease.

#Swift #Codable