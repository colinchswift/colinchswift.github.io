---
layout: post
title: "Serialization and deserialization of complex value types with Codable"
description: " "
date: 2023-09-22
tags: [TechBlog, Swift]
comments: true
share: true
---

In modern programming, it is common to need to convert complex value types, such as structs or classes, into a format that can be stored or transmitted. One way to achieve this in Swift is through serialization and deserialization using `Codable`.

`Codable` is a protocol introduced in Swift 4 that provides a convenient way to encode and decode types to and from external representations, such as JSON or property lists. It allows developers to define a mapping between their complex value types and a serialized representation, enabling easy conversion between the two.

## Serialization with Codable

To serialize a complex value type, first, ensure that it conforms to the `Codable` protocol. This requires implementing the `Encodable` protocol, which defines the encoding process.

```swift
struct Person: Codable {
    let name: String
    let age: Int
}
```

In the above example, the `Person` struct conforms to `Codable` by adopting the protocol. Since all properties in `Person` are of standard types like `String` and `Int`, no additional work is needed.

To serialize an instance of `Person` into JSON, you can use the `JSONEncoder`:

```swift
let person = Person(name: "John Doe", age: 30)

let encoder = JSONEncoder()
encoder.outputFormatting = .prettyPrinted

if let jsonData = try? encoder.encode(person) {
    if let jsonString = String(data: jsonData, encoding: .utf8) {
        print(jsonString)
    }
}
```

This will produce the following output:

```json
{
  "name" : "John Doe",
  "age" : 30
}
```

## Deserialization with Codable

To deserialize a serialized representation back into an instance of a complex value type, the type must conform to `Codable` and implement the `Decodable` protocol. This defines the decoding process.

```swift
struct Person: Codable {
    let name: String
    let age: Int
}
```

In the example above, since `Person` already conforms to `Codable`, there's no further action needed.

To deserialize the JSON representation back into a `Person` instance, use the `JSONDecoder`:

```swift
let jsonString = """
{
    "name": "John Doe",
    "age": 30
}
"""

if let jsonData = jsonString.data(using: .utf8) {
    let decoder = JSONDecoder()

    if let person = try? decoder.decode(Person.self, from: jsonData) {
        print(person)
    }
}
```

This will print:

```
Person(name: "John Doe", age: 30)
```

## Conclusion

Serialization and deserialization of complex value types with `Codable` in Swift provide a simple and powerful way to convert your types to and from external representations. By conforming to `Codable`, you enable encoding and decoding your types without having to write tedious boilerplate code. This helps make your code clean, concise, and maintainable.

#TechBlog #Swift