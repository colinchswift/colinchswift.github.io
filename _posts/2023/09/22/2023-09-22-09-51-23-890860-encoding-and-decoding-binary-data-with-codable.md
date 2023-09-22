---
layout: post
title: "Encoding and decoding binary data with Codable"
description: " "
date: 2023-09-22
tags: [Swift]
comments: true
share: true
---

Binary data encoding and decoding involves converting data to a format that is suitable for binary representation and vice versa. In Swift, you can use the `Codable` protocol to simplify this process.

### What is Codable?

`Codable` is a protocol introduced in Swift 4 that combines the functionalities of `Encodable` and `Decodable` protocols. It provides an easy way to encode Swift types to different formats, such as JSON or binary data, and decode them back to Swift types.

### Encoding Binary Data

To encode Swift types into binary data, you need to create a custom type that conforms to `Codable`. Let's consider an example where we want to encode and decode a `Person` struct that contains a name and age.

First, define the `Person` struct:

```swift
struct Person: Codable {
    var name: String
    var age: Int
}
```

To encode an instance of `Person` to binary data, you can use the `PropertyListEncoder` class from the `Foundation` framework. Here's an example:

```swift
let person = Person(name: "John Doe", age: 30)

let encoder = PropertyListEncoder()
encoder.outputFormat = .binary

if let data = try? encoder.encode(person) {
    // Use the binary data here
}
```

In the example above, we create an instance of `Person` and initialize a `PropertyListEncoder`. We set the `outputFormat` of the encoder to `.binary` to generate binary data. Then, we use the `encode(_:)` function to convert the `person` object to binary data.

### Decoding Binary Data

Conversely, to decode binary data back to Swift types, use the `PropertyListDecoder` class. Consider the following example:

```swift
let binaryData: Data = ...

let decoder = PropertyListDecoder()

if let person = try? decoder.decode(Person.self, from: binaryData) {
    // Access person.name and person.age here
}
```

In the example above, we assume that you have binary data stored in the `binaryData` variable. We create a `PropertyListDecoder` and use the `decode(_:from:)` function to convert the binary data back to a `Person` instance. You can then access the properties of the decoded object.

### Conclusion

Swift's `Codable` protocol provides a convenient way to encode and decode Swift types to binary data. By using `PropertyListEncoder` and `PropertyListDecoder`, you can easily convert your custom types to and from binary data. This allows for efficient transfer and storage of data in binary format.

#iOS #Swift