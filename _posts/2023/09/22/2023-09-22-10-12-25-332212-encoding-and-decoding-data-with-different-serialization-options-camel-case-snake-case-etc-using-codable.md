---
layout: post
title: "Encoding and decoding data with different serialization options (camel case, snake case, etc.) using Codable"
description: " "
date: 2023-09-22
tags: [coding, swift]
comments: true
share: true
---

In modern app development, data serialization and deserialization are important operations. Codable, introduced in Swift 4, provides an easy and convenient way to encode and decode Swift types. In this blog post, we will explore how to encode and decode data with different serialization options such as camel case and snake case using Codable.

## What is Codable?

Codable is a protocol in Swift that allows for easy serialization and deserialization of Swift types. It combines the functionality of the `Encodable` and `Decodable` protocols, making it simple to encode and decode data using various formats.

## Camel Case Encoding and Decoding

By default, Codable uses camel case (lower camel case to be precise) for encoding and decoding properties. Camel case represents multi-word identifiers by capitalizing the first letter of each word (except the first word) and removing any spaces or punctuation.

Here's an example of a simple `Person` struct that conforms to Codable:

```swift
struct Person: Codable {
    let firstName: String
    let lastName: String
    let age: Int
}
```

To encode an instance of `Person` to JSON with camel case keys, we can use the `JSONEncoder`:

```swift
let person = Person(firstName: "John", lastName: "Doe", age: 25)

let encoder = JSONEncoder()
encoder.keyEncodingStrategy = .convertToSnakeCase

let jsonData = try encoder.encode(person)
let jsonString = String(data: jsonData, encoding: .utf8)
print(jsonString ?? "")
```

In this example, we explicitly set the `keyEncodingStrategy` of the encoder to `.convertToSnakeCase`, which converts camel case keys to snake case keys (using underscores to separate words). The output JSON will look like this:

```json
{
    "first_name": "John",
    "last_name": "Doe",
    "age": 25
}
```

To decode JSON data with camel case keys back into a `Person` object, we use the `JSONDecoder` and set the `keyDecodingStrategy` to `.convertFromSnakeCase`:

```swift
let jsonString = """
{
    "first_name": "John",
    "last_name": "Doe",
    "age": 25
}
"""

let decoder = JSONDecoder()
decoder.keyDecodingStrategy = .convertFromSnakeCase

if let jsonData = jsonString.data(using: .utf8) {
    let person = try decoder.decode(Person.self, from: jsonData)
    print(person)
}
```

This will correctly decode the JSON data and create a `Person` instance with the values from the JSON.

## Snake Case Encoding and Decoding

In some cases, you may need to work with APIs or systems that use snake case for serialization. Codable makes it easy to handle this scenario as well.

To encode a Swift type with snake case keys, we can again use the `keyEncodingStrategy` property of the `JSONEncoder`:

```swift
let person = Person(firstName: "John", lastName: "Doe", age: 25)

let encoder = JSONEncoder()
encoder.keyEncodingStrategy = .convertToSnakeCase

let jsonData = try encoder.encode(person)
let jsonString = String(data: jsonData, encoding: .utf8)
print(jsonString ?? "")
```

The resulting JSON will use snake case keys:

```json
{
    "first_name": "John",
    "last_name": "Doe",
    "age": 25
}
```

To decode JSON with snake case keys back into a Swift type, we use the `JSONDecoder` and set the `keyDecodingStrategy`:

```swift
let jsonString = """
{
    "first_name": "John",
    "last_name": "Doe",
    "age": 25
}
"""

let decoder = JSONDecoder()
decoder.keyDecodingStrategy = .convertFromSnakeCase

if let jsonData = jsonString.data(using: .utf8) {
    let person = try decoder.decode(Person.self, from: jsonData)
    print(person)
}
```

By setting the `keyDecodingStrategy` to `.convertFromSnakeCase`, the JSON is decoded back into a `Person` object with camel case property names.

## Conclusion

In this blog post, we explored how to encode and decode data with different serialization options using Codable in Swift. We saw how to handle camel case and snake case serialization by utilizing the key encoding and decoding strategies of the `JSONEncoder` and `JSONDecoder` classes. Codable makes it easy to work with various data formats and simplifies the process of data serialization and deserialization in Swift applications.

#coding #swift