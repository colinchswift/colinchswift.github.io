---
layout: post
title: "Customizing JSON serialization in Swift"
description: " "
date: 2023-09-27
tags: [JSON]
comments: true
share: true
---

With the rise of web applications and APIs, JSON (JavaScript Object Notation) has become a ubiquitous format for data exchange. In Swift, the `Codable` protocol provides a convenient way to serialize and deserialize JSON data. However, there are times when you may need to customize the serialization process to suit your specific needs.

## Implementing Codable

Before we dive into customization, let's quickly review the basics of Codable. Codable is a protocol that combines both `Encodable` and `Decodable` protocols, which are used for serialization and deserialization, respectively. By conforming your custom types to Codable, Swift automatically generates the necessary code for JSON serialization and deserialization.

Here's an example of a basic `User` struct that conforms to Codable:

```swift
struct User: Codable {
    let name: String
    let age: Int
    let email: String
}
```

By default, all properties of the struct will be serialized and deserialized using their names as keys in the JSON data. 

## Customizing Key Names

To customize the key names during serialization and deserialization, you can define an `enum` that conforms to `CodingKey` within your struct. This allows you to map your custom names to the actual property names.

```swift
struct User: Codable {
    let name: String
    let age: Int
    let email: String
    
    enum CodingKeys: String, CodingKey {
        case name = "full_name"
        case age
        case email
    }
}
```

In the example above, we've provided custom key names for `name` property. This will ensure that the JSON key will be `"full_name"` when serialized, instead of `"name"`.

## Excluding Properties

Sometimes you might want to exclude certain properties from serialization or deserialization. For this, you can use the `CodingKeys` enum and simply omit the properties you don't want to include.

```swift
struct User: Codable {
    let name: String
    let age: Int
    let email: String
    let password: String
    
    enum CodingKeys: String, CodingKey {
        case name = "full_name"
        case age
        case email
    }
}
```

In the above example, the `password` property will not be serialized or deserialized since it is not included in the `CodingKeys` enum.

## Custom Encoding and Decoding

For fine-grained control over the serialization and deserialization process, you can implement the `encode(to:)` and `init(from:)` methods manually. These methods allow you to manually encode or decode each property, performing additional transformations if needed.

```swift
struct User: Codable {
    let name: String
    let age: Int
    let email: String
    
    enum CodingKeys: String, CodingKey {
        case name = "full_name"
        case age
        case email
    }
    
    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(name.uppercased(), forKey: .name)
        try container.encode(age * 2, forKey: .age)
        try container.encode(email.lowercased(), forKey: .email)
    }
    
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        name = try container.decode(String.self, forKey: .name).lowercased()
        age = try container.decode(Int.self, forKey: .age) / 2
        email = try container.decode(String.self, forKey: .email).uppercased()
    }
}
```

In the above example, we've customized the encoding and decoding process by applying different transformations to each property. The `name` is encoded as uppercase and decoded as lowercase, the `age` is doubled during encoding and halved during decoding, and the `email` is encoded as lowercase and decoded as uppercase.

## Conclusion

Customizing JSON serialization in Swift provides flexibility and control over the data exchange process. By leveraging the power of Codable and the CodingKeys enum, you can easily customize key names, exclude properties, and even perform complex encoding and decoding transformations. This allows you to handle various scenarios and adapt to different API requirements seamlessly.

#Swift #JSON #CodingKeys