---
layout: post
title: "Configuring custom serializers and deserializers with Codable"
description: " "
date: 2023-09-22
tags: [Swift, Codable]
comments: true
share: true
---

When working with Codable in Swift, we often rely on its built-in serialization and deserialization capabilities. However, there are situations where we need custom logic for handling specific data types or formats. In these cases, we can configure custom serializers and deserializers with Codable to handle those scenarios.

## Serializers

Serializers are responsible for converting Swift objects into an encoded representation, typically JSON or binary data. To create a custom serializer, we need to implement the `encode(to: Encoder)` method in our Codable types.

Here's an example of a custom serializer for a `Person` type:

```swift
struct Person: Codable {
    let name: String
    let age: Int

    // Custom serializer
    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(name.uppercased(), forKey: .name)
        try container.encode(age + 1, forKey: .age)
    }

    enum CodingKeys: String, CodingKey {
        case name
        case age
    }
}
```

In the above example, the `encode(to: Encoder)` method is overridden to customize the serialization process. In this case, we uppercase the name and increment the age by one before encoding the values.

## Deserializers

Deserializers, on the other hand, handle the process of converting encoded data back into Swift objects. To create a custom deserializer, we need to implement the `init(from: Decoder)` initializer in our Codable types.

Let's continue with the `Person` example and create a custom deserializer:

```swift
struct Person: Codable {
    let name: String
    let age: Int

    // Custom deserializer
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        name = try container.decode(String.self, forKey: .name).lowercased()
        age = try container.decode(Int.self, forKey: .age) - 1
    }

    enum CodingKeys: String, CodingKey {
        case name
        case age
    }
}
```

In the above code snippet, the `init(from: Decoder)` initializer is implemented with custom logic for deserialization. We lowercase the name and subtract one from the age after decoding the values.

## Using Custom Serializers and Deserializers

To use our custom serializers and deserializers, we simply encode or decode our Codable types as usual:

```swift
// Encoding example
let person = Person(name: "John Smith", age: 30)
let jsonEncoder = JSONEncoder()
jsonEncoder.outputFormatting = .prettyPrinted
let jsonData = try jsonEncoder.encode(person)
let jsonString = String(data: jsonData, encoding: .utf8)
print(jsonString ?? "")

// Decoding example
let jsonString = """
{
    "name": "JANE DOE",
    "age": 25
}
"""
let jsonDecoder = JSONDecoder()
let decodedPerson = try jsonDecoder.decode(Person.self, from: jsonString.data(using: .utf8)!)
print(decodedPerson)
```

In the encoding example, the custom serializer will uppercase the name and increment the age by one. In the decoding example, the custom deserializer will lowercase the name and subtract one from the age.

By configuring custom serializers and deserializers with Codable, we can handle specific data transformations or formats that don't fit the default behavior. This flexibility allows us to work with various APIs and data sources more effectively.

#Swift #Codable