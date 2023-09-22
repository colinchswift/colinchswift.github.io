---
layout: post
title: "Encoding and decoding structured data formats (JSON Schema, Protocol Buffers, etc.) with Codable"
description: " "
date: 2023-09-22
tags: [Swift, Codable]
comments: true
share: true
---

In modern software development, we often need to work with structured data formats like JSON Schema, Protocol Buffers, XML, and more. These formats allow us to define the structure and rules of our data, making it easier to share and consume data between different systems. 

One powerful feature introduced in Swift 4 is Codable, a protocol that allows you to encode and decode Swift types to and from external representations, such as JSON or binary formats. It provides a seamless way to convert your Swift objects to and from different data formats, making data serialization and deserialization a breeze.

## How Codable works

Codable is actually a composition of two protocols: `Encodable` and `Decodable`. By conforming your custom types to these protocols, you can encode and decode your objects using the built-in functionality provided by Codable. 

### Encoding with Codable

To encode an object using Codable, you need to make your custom type conform to the `Encodable` protocol. This means implementing the `encode(to:)` method, which encodes your object to an external representation like JSON or binary data. 

Here's an example:

```swift
struct Person: Codable {
    var name: String
    var age: Int
}

let person = Person(name: "John", age: 30)

let encoder = JSONEncoder()
if let encodedData = try? encoder.encode(person) {
    let jsonString = String(data: encodedData, encoding: .utf8)
}
```

In the example above, we define a `Person` struct that conforms to `Codable` by adopting the `Encodable` protocol. We then create an instance of `Person` and use a `JSONEncoder` to encode it to JSON data. Finally, we convert the encoded data to a string for display.

### Decoding with Codable

Decoding works similarly to encoding. To decode an external representation into your custom type, you need to conform your type to the `Decodable` protocol. This involves implementing the `init(from:)` initializer, which takes an instance of `Decoder` and decodes the data.

Here's an example:

```swift
let json = """
{
    "name": "Jane",
    "age": 25
}
"""

let jsonData = json.data(using: .utf8)

let decoder = JSONDecoder()
if let decodedPerson = try? decoder.decode(Person.self, from: jsonData) {
    print(decodedPerson)
}
```

In the above example, we have a JSON string representing a `Person` object. We convert the JSON string to `Data` and then use a `JSONDecoder` to decode it into a `Person` object. If the decoding succeeds, we can access the decoded object just like any other Swift object.

## Conclusion

Codable provides a convenient and powerful way to encode and decode Swift types to and from external data formats. By conforming to the `Encodable` and `Decodable` protocols, you can seamlessly serialize and deserialize your data, making it easier to work with different structured data formats in your applications.

# #Swift #Codable