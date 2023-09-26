---
layout: post
title: "Using Codable for automatic serialization and deserialization of data models"
description: " "
date: 2023-09-22
tags: [Codable]
comments: true
share: true
---

In modern software development, handling data serialization and deserialization is a common task. This process involves converting data objects into a format that can be easily stored or transmitted, such as JSON or XML, and reconstructing them back into objects when needed. Codable, introduced in Swift 4, is a powerful protocol that provides a simple and elegant solution for automatic serialization and deserialization of data models.

## What is Codable?

Codable is a protocol in Swift that combines both the `Encodable` and `Decodable` protocols. By adopting the `Codable` protocol, you can effortlessly encode and decode objects without having to write complex serialization and deserialization code.

## Declaring Codable Data Models

To make a data model conform to the `Codable` protocol, simply adopt the protocol and declare the properties that need to be serialized and deserialized.

Here's an example of a `Person` data model:

```swift
struct Person: Codable {
    var name: String
    var age: Int
}
```

In this example, the `Person` struct conforms to the `Codable` protocol by simply adding `Codable` after the struct's name. The `name` and `age` properties will automatically be serialized and deserialized.

## Encoding Data Objects

To encode a data object into a specific format, such as JSON, use an instance of `JSONEncoder`:

```swift
let person = Person(name: "John Doe", age: 30)

let encoder = JSONEncoder()

do {
    let jsonData = try encoder.encode(person)
    let jsonString = String(data: jsonData, encoding: .utf8)
    // jsonString now contains the serialized JSON representation of the person object
} catch {
    print("Error encoding person: \(error.localizedDescription)")
}
```

In this example, a `person` object is encoded into JSON format using an instance of `JSONEncoder`. The resulting `jsonData` can be converted to a string representation, `jsonString`, using the `.utf8` encoding.

## Decoding Data Objects

To decode a data object from a specific format, such as JSON, use an instance of `JSONDecoder`:

```swift
let jsonString = """
{
    "name": "John Doe",
    "age": 30
}
"""

let decoder = JSONDecoder()

do {
    let jsonData = Data(jsonString.utf8)
    let person = try decoder.decode(Person.self, from: jsonData)
    // The person object is now deserialized from the JSON representation
} catch {
    print("Error decoding person: \(error.localizedDescription)")
}
```

In this example, a `jsonString` containing a JSON representation of a person object is decoded using an instance of `JSONDecoder`. The decoded `person` object is of type `Person` and can be used in your application.

## Customizing Encoding and Decoding

The default serialization and deserialization provided by `Codable` may not always meet your specific requirements. In such cases, you can customize the encoding and decoding process by implementing the `CodingKey` protocol and using its associated values.

For example, you can use the `CodingKeys` enum to map your data model properties to different keys in the serialized representation:

```swift
struct Person: Codable {
    var name: String
    var age: Int
    
    enum CodingKeys: String, CodingKey {
        case name = "full_name"
        case age
    }
}
```

In this example, the `name` property is now mapped to the JSON key `"full_name"` instead of `"name"`.

## Conclusion

Using the `Codable` protocol in Swift can significantly simplify the process of serializing and deserializing data models. By adopting `Codable`, you can eliminate the need for manually writing serialization and deserialization code, making your codebase more concise and maintainable. Remember to customize the process when necessary using the `CodingKey` protocol. Start leveraging the power of `Codable` in your iOS or macOS apps today!

#Swift #Codable