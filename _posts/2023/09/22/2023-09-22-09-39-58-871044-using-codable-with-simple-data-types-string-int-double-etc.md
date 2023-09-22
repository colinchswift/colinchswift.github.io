---
layout: post
title: "Using Codable with simple data types (String, Int, Double, etc.)"
description: " "
date: 2023-09-22
tags: [Swift, Coding]
comments: true
share: true
---

In Swift, the `Codable` protocol provides an easy way to encode and decode data to and from various formats, such as JSON or property lists. While `Codable` is commonly used for more complex data structures, it can also be used with simple data types like `String`, `Int`, `Double`, and so on.

## Encoding simple data types

To encode simple data types using `Codable`, you need to create a struct or a class that conforms to the `Codable` protocol. Let's take a look at an example where we want to encode a `Person` object with a `name`, `age`, and `height`:

```swift
struct Person: Codable {
    let name: String
    let age: Int
    let height: Double
}

let person = Person(name: "John Doe", age: 30, height: 180.5)

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

In the above example, we create a `Person` struct that conforms to the `Codable` protocol. We then create an instance of `Person` with some values. We use a `JSONEncoder` to encode the `person` object into a JSON data representation. Finally, we convert the JSON data to a string for printing.

## Decoding simple data types

Decoding simple data types is quite similar to encoding. We create a struct or a class that conforms to `Codable`, and use a `JSONDecoder` to decode the data. Here's an example of decoding a JSON string into a `Person` object:

```swift
let jsonString = """
    {
        "name": "John Doe",
        "age": 30,
        "height": 180.5
    }
"""

let decoder = JSONDecoder()

do {
    let data = jsonString.data(using: .utf8)
    let person = try decoder.decode(Person.self, from: data!)
    print(person)
} catch {
    print("Decoding failed: \(error)")
}
```

In the above example, we have a JSON string that represents a `Person` object. We create a `JSONDecoder` and use it to decode the JSON data into a `Person` object.

## Conclusion

Using `Codable` with simple data types in Swift allows for easy encoding and decoding of data to and from various formats. By conforming to the `Codable` protocol and using the appropriate encoders and decoders, you can handle simple data types with ease. #Swift #Coding