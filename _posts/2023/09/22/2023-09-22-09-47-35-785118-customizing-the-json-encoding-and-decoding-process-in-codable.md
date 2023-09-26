---
layout: post
title: "Customizing the JSON encoding and decoding process in Codable"
description: " "
date: 2023-09-22
tags: [Codable]
comments: true
share: true
---

Codable is a powerful Swift feature that allows us to easily convert Swift objects to JSON and vice versa. By default, Codable automatically encodes and decodes properties that conform to the Codable protocol using JSON's key-value pairs. However, there might be cases where we need more control over the encoding and decoding process, such as handling custom date formats or mapping different property names.

In this blog post, we will explore how to customize the JSON encoding and decoding process in Codable by implementing custom coding keys and writing custom encoding and decoding methods.

## Custom Coding Keys

When the property names in your Swift object don't match the key names in the JSON, you can use custom coding keys to define the mapping between them.

Example:

```swift
struct Person: Codable {
    let firstName: String
    let lastName: String
    let dateOfBirth: Date

    private enum CodingKeys: String, CodingKey {
        case firstName = "first_name"
        case lastName = "last_name"
        case dateOfBirth = "birth_date"
    }
}
```

In the example above, we define a `CodingKeys` enumeration with string raw values that represent the key names in the JSON. By assigning each Swift property to a corresponding coding key, Codable knows how to map the JSON keys to the Swift object's properties correctly.

## Custom Encoding and Decoding Methods

Sometimes, you might need to perform additional processing during the encoding and decoding process. For example, you might want to convert the date format used in your Swift object, or apply some custom logic to a property.

To achieve this, Codable provides two optional methods: `func encode(to encoder: Encoder) throws` and `init(from decoder: Decoder) throws`. By implementing these methods, we can perform our custom encoding and decoding operations.

Example:

```swift
struct Person: Codable {
    let firstName: String
    let lastName: String
    let dateOfBirth: Date

    // ... other properties and custom coding keys

    private enum CodingKeys: String, CodingKey {
        // ... coding keys
    }

    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(firstName, forKey: .firstName)
        try container.encode(lastName.uppercased(), forKey: .lastName)
        let dateFormatter = DateFormatter()
        dateFormatter.dateFormat = "yyyy-MM-dd"
        let birthDate = dateFormatter.string(from: dateOfBirth)
        try container.encode(birthDate, forKey: .dateOfBirth)
    }

    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        firstName = try container.decode(String.self, forKey: .firstName)
        lastName = try container.decode(String.self, forKey: .lastName)
        let birthDateString = try container.decode(String.self, forKey: .dateOfBirth)
        let dateFormatter = DateFormatter()
        dateFormatter.dateFormat = "yyyy-MM-dd"
        dateOfBirth = dateFormatter.date(from: birthDateString) ?? Date()
    }
}
```

In the example above, we override the `encode(to:)` and `init(from:)` methods to define our custom encoding and decoding logic. We uppercase the `lastName`, convert the `dateOfBirth` to a custom date format, and handle any errors that might occur during the encoding and decoding process.

## Conclusion

Customizing the JSON encoding and decoding process in Codable allows us to handle complex scenarios where the default behavior doesn't suffice. By using custom coding keys and implementing custom encoding and decoding methods, we take control of how our Swift objects are converted to and from JSON. This flexibility makes Codable a versatile tool for working with JSON data in Swift.

#Swift #Codable #JSON