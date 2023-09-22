---
layout: post
title: "Implementing custom string encoding and decoding for textual data using Codable"
description: " "
date: 2023-09-22
tags: []
comments: true
share: true
---

With the introduction of `Codable` in Swift, encoding and decoding data structures has become much easier. However, sometimes we may need to implement custom encoding and decoding logic for specific data types. In this blog post, we will explore how to implement custom string encoding and decoding for textual data using `Codable`.

## Encoding

To implement custom encoding for a textual data type, we need to conform to the `Encodable` protocol. Let's assume we have a `Person` struct with a `name` property of type `String`. We want to encode the `name` property by converting it to uppercase before encoding it as a string. Here's an example of how we can achieve this:

```swift
struct Person: Codable {
    var name: String

    enum CodingKeys: String, CodingKey {
        case name
    }

    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(name.uppercased(), forKey: .name)
    }
}

let person = Person(name: "John Doe")
let encoder = JSONEncoder()
let encodedData = try encoder.encode(person)
let encodedString = String(data: encodedData, encoding: .utf8)
```

In the `encode(to:)` method, we create a container keyed by our `CodingKeys` enum and encode the uppercase version of the `name` property using the `name` key. Finally, we can use a `JSONEncoder` to encode the `Person` instance into data and get the encoded string representation.

## Decoding

To implement custom decoding for a textual data type, we need to conform to the `Decodable` protocol. Let's extend our `Person` struct from the previous example to decode a string to lowercase for the `name` property. Here's an example of how we can achieve this:

```swift
struct Person: Codable {
    var name: String

    enum CodingKeys: String, CodingKey {
        case name
    }

    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        name = try container.decode(String.self, forKey: .name)
        name = name.lowercased()
    }
}

let jsonString = """
{
    "name": "JOHN DOE"
}
"""

let jsonData = jsonString.data(using: .utf8)!
let decoder = JSONDecoder()
let decodedPerson = try decoder.decode(Person.self, from: jsonData)
```

In the `init(from:)` initializer, we decode the `name` property using the `name` key and then convert it to lowercase before assigning it to the `name` property.

## Conclusion

By implementing custom encoding and decoding logic, we can customize how textual data is represented in our JSON or other encoded formats using `Codable`. This can be useful in scenarios where we need to transform or normalize the data during the encoding and decoding process.