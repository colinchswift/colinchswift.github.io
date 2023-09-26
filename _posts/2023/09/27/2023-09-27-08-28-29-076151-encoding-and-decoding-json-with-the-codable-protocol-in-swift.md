---
layout: post
title: "Encoding and decoding JSON with the Codable protocol in Swift"
description: " "
date: 2023-09-27
tags: []
comments: true
share: true
---

JSON (JavaScript Object Notation) is a popular data interchange format used for communication between a client and server. In Swift, encoding and decoding JSON can be done using the `Codable` protocol, introduced in Swift 4. The `Codable` protocol provides an easy and efficient way to convert Swift objects to and from JSON.

## The Codable protocol

The `Codable` protocol is a combination of the `Decodable` and `Encodable` protocols. Conforming to `Codable` allows an object to be encoded to a JSON representation or decoded from a JSON representation.

When defining a Swift struct or class, you can adopt the `Codable` protocol to enable encoding and decoding functionality automatically.

## Encoding JSON using the Codable protocol

To encode a Swift object to JSON, follow these steps:

1. Define your Swift struct or class and make it conform to `Codable`.
2. Create an instance of your struct or class.
3. Use the `JSONEncoder` class to encode the object, and handle any errors that may occur.
4. Convert the encoded data to a JSON string.

Here's an example of encoding a `Person` struct to JSON:

```swift
struct Person: Codable {
    var name: String
    var age: Int
}

let person = Person(name: "John Appleseed", age: 30)
let encoder = JSONEncoder()

do {
    let jsonData = try encoder.encode(person)
    let jsonString = String(data: jsonData, encoding: .utf8)
    print(jsonString ?? "")
} catch {
    print("Error encoding JSON: \(error)")
}
```

In the above code, we define a `Person` struct that conforms to `Codable`. We create an instance of `Person`, and then use the `JSONEncoder` to encode it to JSON data. Finally, we convert the data to a JSON string using the `.utf8` encoding.

## Decoding JSON using the Codable protocol

To decode JSON into a Swift object, follow these steps:

1. Define your Swift struct or class and make it conform to `Codable`.
2. Get the JSON data from a source, such as an API response or a file.
3. Use the `JSONDecoder` class to decode the JSON data into your Swift object, and handle any errors that may occur.

Here's an example of decoding JSON into a `Person` struct:

```swift
let jsonString = """
{
    "name": "Jane Doe",
    "age": 25
}
"""

let jsonData = jsonString.data(using: .utf8)
let decoder = JSONDecoder()

do {
    let person = try decoder.decode(Person.self, from: jsonData!)
    print(person)
} catch {
    print("Error decoding JSON: \(error)")
}
```

In the above code, we have a JSON string representing a `Person` object. We convert the JSON string to data, and then use the `JSONDecoder` to decode it into a `Person` instance.

## Conclusion

The `Codable` protocol in Swift makes encoding and decoding JSON a breeze. By adopting `Codable` in your structs and classes, you can easily convert between JSON and Swift objects without the need for manual serialization and deserialization.