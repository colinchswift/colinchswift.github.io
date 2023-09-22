---
layout: post
title: "Understanding the JSONEncoder and JSONDecoder classes in Codable"
description: " "
date: 2023-09-22
tags: [json, encoding]
comments: true
share: true
---

In Swift, the `Codable` protocol provides an easy way to encode and decode data in various formats, including JSON. The `JSONEncoder` and `JSONDecoder` classes are two important components of the `Codable` protocol that allow us to perform encoding and decoding operations.

## JSONEncoder

The `JSONEncoder` class is responsible for converting a Swift data structure or object into a JSON representation. It provides several encoding strategies to customize the output format, such as `.convertToSnakeCase` for converting camel case property names to snake case.

To use the `JSONEncoder`, we first need to create an instance of it and then call its `encode(_:)` method, passing in the object we want to encode. Here's an example:

```swift
struct Person: Codable {
    var name: String
    var age: Int
}

let person = Person(name: "John Doe", age: 30)
let encoder = JSONEncoder()
encoder.outputFormatting = .prettyPrinted

do {
    let jsonData = try encoder.encode(person)
    if let jsonString = String(data: jsonData, encoding: .utf8) {
        print(jsonString)
    }
} catch {
    print("Encoding failed: \(error)")
}
```

In this example, we define a `Person` struct that conforms to `Codable`. We create an instance of `Person` and an instance of `JSONEncoder`. We set the `outputFormatting` property to `.prettyPrinted` to make the JSON output more readable. Then, we call `encode(_:)` method on the `JSONEncoder` instance and convert the resulting `Data` to a `String` for printing.

## JSONDecoder

The `JSONDecoder` class is used to convert JSON data back into Swift data structures or objects. It provides various decoding strategies to handle different naming conventions, such as `.convertFromSnakeCase` for converting snake case key names to camel case.

To use the `JSONDecoder`, we create an instance of it and then call its `decode(_:from:)` method, passing in the type we want to decode and the JSON data. Here's an example:

```swift
let jsonString = """
{
    "name": "Jane Smith",
    "age": 25
}
"""

let decoder = JSONDecoder()

do {
    let jsonData = jsonString.data(using: .utf8)!
    let person = try decoder.decode(Person.self, from: jsonData)
    print(person)
} catch {
    print("Decoding failed: \(error)")
}
```

In this example, we have a JSON string representing a `Person`. We create an instance of `JSONDecoder` and then call `decode(_:from:)` method, passing in the `Person` type and the JSON data. The resulting `Person` object is printed to the console.

#json #encoding #decoding