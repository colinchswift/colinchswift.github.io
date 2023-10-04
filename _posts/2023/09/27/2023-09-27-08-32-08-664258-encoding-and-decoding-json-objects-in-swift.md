---
layout: post
title: "Encoding and decoding JSON objects in Swift"
description: " "
date: 2023-09-27
tags: [Coding]
comments: true
share: true
---

JSON (JavaScript Object Notation) is a lightweight data-interchange format that is easy for humans to read and write, and easy for machines to parse and generate. In Swift, we can easily encode and decode JSON objects using the `Codable` protocol.

## Encoding JSON Objects

To encode a Swift object into a JSON representation, follow these steps:

1. Define a structure or class that conforms to the `Codable` protocol. Use the `Codable` protocol to mark the properties that should be encoded and decoded.

```swift
struct Person: Codable {
    var name: String
    var age: Int
    var email: String
}
```

2. Create an instance of the object that you want to encode.

```swift
let person = Person(name: "John Doe", age: 30, email: "johndoe@example.com")
```

3. Use the `JSONEncoder` class to encode the object into JSON data.

```swift
let encoder = JSONEncoder()
encoder.outputFormatting = .prettyPrinted

do {
    let jsonData = try encoder.encode(person)
    if let jsonString = String(data: jsonData, encoding: .utf8) {
        print(jsonString)
    }
} catch {
    print("Error encoding JSON: \(error)")
}
```

The `JSONEncoder` class converts the object to JSON data. By setting `outputFormatting` to `.prettyPrinted`, the output will be formatted in a human-readable way.

## Decoding JSON Objects

To decode a JSON object into a Swift object, follow these steps:

1. Define a structure or class that conforms to the `Codable` protocol, matching the structure of the JSON you want to decode.

```swift
struct Person: Codable {
    var name: String
    var age: Int
    var email: String
}
```

2. Get the JSON data from an external source, such as a web API.

3. Use the `JSONDecoder` class to decode the JSON data into a Swift object.

```swift
let jsonString = """
{
    "name": "John Doe",
    "age": 30,
    "email": "johndoe@example.com"
}
"""

let decoder = JSONDecoder()

do {
    let jsonData = jsonString.data(using: .utf8)!
    let person = try decoder.decode(Person.self, from: jsonData)
    print(person)
} catch {
    print("Error decoding JSON: \(error)")
}
```

The `JSONDecoder` class converts the JSON data into a Swift object. We use the `decode(_:from:)` method, passing in the type of the Swift object that we expect to decode (`Person.self` in this example).

## Conclusion

Encoding and decoding JSON objects in Swift is made easy with the `Codable` protocol, `JSONEncoder`, and `JSONDecoder` classes. By following the steps above, you can quickly convert between Swift objects and JSON representations, allowing for seamless integration with web APIs and data exchange between different systems.

#Coding #Swift