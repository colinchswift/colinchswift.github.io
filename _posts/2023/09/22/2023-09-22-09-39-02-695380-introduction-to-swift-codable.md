---
layout: post
title: "Introduction to Swift Codable"
description: " "
date: 2023-09-22
tags: [codable, swift]
comments: true
share: true
---

In Swift, the **Codable** protocol provides a convenient way to encode and decode data to and from different representations, such as JSON or Property List. Codable is an important concept to understand when working with network requests, persistence, or any operation that involves serializing and deserializing data.

## What is Codable?

**Codable** is a protocol introduced in Swift 4 that combines two other protocols: **Encodable** and **Decodable**. 

- **Encodable**: This protocol defines an object that can be encoded (converted to an external representation, such as JSON or Property List). It requires implementing a single method, `encode(to: Encoder)`, which is responsible for encoding the object.

- **Decodable**: This protocol defines an object that can be decoded (converted from an external representation, such as JSON or Property List, to an actual instance of the object). It requires implementing a single initializer, `init(from: Decoder)`, which is responsible for decoding the object.

By adopting the Codable protocol, a Swift type supports both encoding and decoding operations in a standardized way.

## Using Codable

To use Codable in Swift, you simply need to adopt the Codable protocol in your custom types and structs. For example, consider the following `Person` struct:

```swift
struct Person: Codable {
    let name: String
    let age: Int
}
```

With just a single line of code `struct Person: Codable`, the `Person` struct now supports both encoding and decoding.

### Encoding

To encode a Codable object to a data representation (e.g., JSON), you can use an instance of the JSONEncoder class. Here's an example:

```swift
let person = Person(name: "John Doe", age: 30)
let encoder = JSONEncoder()
encoder.outputFormatting = .prettyPrinted

do {
    let data = try encoder.encode(person)
    let jsonString = String(data: data, encoding: .utf8)
    print(jsonString ?? "")
} catch {
    print("Encoding failed: \(error)")
}
```

In the example above, we create an instance of `JSONEncoder`, configure its `outputFormatting` property to produce a human-readable JSON, and encode the `person` object. The resulting data is then converted to a String representation and printed.

### Decoding

To decode a data representation (e.g., JSON) into a Codable object, you can use an instance of the JSONDecoder class. Here's an example:

```swift
let jsonString = """
{
    "name": "Jane Smith",
    "age": 25
}
"""

let decoder = JSONDecoder()

do {
    let data = jsonString.data(using: .utf8)!
    let person = try decoder.decode(Person.self, from: data)
    print(person)
} catch {
    print("Decoding failed: \(error)")
}
```

In this example, we create an instance of `JSONDecoder`, pass the JSON string data, and use the `decode(_:from:)` method to decode the JSON into a `Person` object.

## Conclusion

Swift Codable is a powerful protocol that simplifies the process of encoding and decoding data in different representations. By adopting the Codable protocol on your custom types, you can easily handle serialization and deserialization operations. This makes it an essential tool when dealing with tasks like network requests, data persistence, or any operation that requires working with external representations of data.

#codable #swift