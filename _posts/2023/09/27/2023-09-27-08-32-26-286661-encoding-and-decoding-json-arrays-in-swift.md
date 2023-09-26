---
layout: post
title: "Encoding and decoding JSON arrays in Swift"
description: " "
date: 2023-09-27
tags: []
comments: true
share: true
---

JSON (JavaScript Object Notation) is a lightweight data interchange format commonly used in web APIs. When working with JSON data in Swift, you may often encounter scenarios where data needs to be encoded into a JSON array or decoded from a JSON array. In this article, we will explore how to encode and decode JSON arrays in Swift using the Codable protocol.

## Encoding JSON Arrays

To encode a Swift array into a JSON array, we need to conform to the `Encodable` protocol and use the `JSONEncoder` class. Here's an example:

```swift
struct Person: Codable {
    let name: String
    let age: Int
}

let persons = [
    Person(name: "John", age: 25),
    Person(name: "Jane", age: 30),
    Person(name: "Mike", age: 40)
]

let encoder = JSONEncoder()
if let jsonData = try? encoder.encode(persons) {
    // Use jsonData as needed
    print(jsonData)
}
```

In the code above, we define a `Person` struct that conforms to the `Codable` protocol. We then create an array of `Person` objects. Next, we create an instance of `JSONEncoder` and use it to encode the array of `Person` objects into a JSON data representation. Finally, we can use the `jsonData` as needed.

## Decoding JSON Arrays

To decode a JSON array into Swift objects, we need to conform to the `Decodable` protocol and use the `JSONDecoder` class. Here's an example:

```swift
let jsonString = """
[
    { "name": "John", "age": 25 },
    { "name": "Jane", "age": 30 },
    { "name": "Mike", "age": 40 }
]
"""

let jsonData = jsonString.data(using: .utf8)

let decoder = JSONDecoder()
if let persons = try? decoder.decode([Person].self, from: jsonData) {
    // Use persons as needed
    print(persons)
}
```

In the code above, we define a JSON string representing a JSON array of `Person` objects. We convert the string to `Data` using the `data(using: .utf8)` method. Next, we create an instance of `JSONDecoder` and use it to decode the JSON data into an array of `Person` objects. Finally, we can use the `persons` array as needed.

## Conclusion

Encoding and decoding JSON arrays in Swift is made easy with the `Codable` protocol and the `JSONEncoder` and `JSONDecoder` classes. By leveraging these tools, you can quickly convert your Swift arrays into JSON arrays and vice versa.