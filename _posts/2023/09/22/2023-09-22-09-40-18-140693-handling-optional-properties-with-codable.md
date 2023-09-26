---
layout: post
title: "Handling optional properties with Codable"
description: " "
date: 2023-09-22
tags: [codable]
comments: true
share: true
---

When working with Codable in Swift, it is common to have properties that are optional in your data model. Optional properties can be useful when certain data may or may not be present. In this blog post, we will explore how to handle optional properties when encoding and decoding data using Codable.

## Encoding Optional Properties

To encode optional properties, you need to conform to the `Encodable` protocol and implement the `encode(to:)` method. Inside the `encode(to:)` method, you can use conditional statements to encode the optional property only if it has a value. Here's an example:

```swift
struct Person: Codable {
    let name: String
    let age: Int?
    
    enum CodingKeys: String, CodingKey {
        case name
        case age
    }
    
    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(name, forKey: .name)
        
        if let age = age {
            try container.encode(age, forKey: .age)
        }
    }
}
```

In this example, the `age` property is optional, indicated by the use of the question mark after the `Int` type. The `encode(to:)` method checks if `age` has a value before encoding it.

## Decoding Optional Properties

When decoding data that has optional properties, you need to conform to the `Decodable` protocol and implement the `init(from:)` initializer. Inside the `init(from:)` initializer, you can handle the optional property by using the `decodeIfPresent(_:forKey:)` method. Here's an example:

```swift
struct Person: Codable {
    let name: String
    let age: Int?
    
    enum CodingKeys: String, CodingKey {
        case name
        case age
    }
    
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        name = try container.decode(String.self, forKey: .name)

        age = try container.decodeIfPresent(Int.self, forKey: .age)
    }
}
```

In this example, the `age` property is decoded using the `decodeIfPresent(_:forKey:)` method, which returns `nil` if the key is not present or the value is not of the expected type.

## Conclusion

Handling optional properties with Codable is straightforward. By implementing the appropriate methods in the `Encodable` and `Decodable` protocols, you can encode and decode optional properties in your data model. This enables you to handle data where certain values may or may not be present, providing flexibility in your code.

#swift #codable