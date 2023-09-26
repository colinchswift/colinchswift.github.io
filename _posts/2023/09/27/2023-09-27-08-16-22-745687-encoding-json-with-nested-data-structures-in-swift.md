---
layout: post
title: "Encoding JSON with nested data structures in Swift"
description: " "
date: 2023-09-27
tags: [Swift, JSON]
comments: true
share: true
---

When working with JSON in Swift, sometimes we need to encode complex nested data structures into JSON format. Swift provides a convenient way to handle this using the `Codable` protocol, introduced in Swift 4.

The `Codable` protocol allows us to easily convert Swift objects to and from JSON by providing a mapping between the object's properties and the JSON keys. In the case of nested data structures, we can use nested structs or classes in Swift to represent the structure.

Let's say we have a data structure representing a person, which contains their basic information and an array of nested objects representing their friends:

```swift
struct Person: Codable {
    let name: String
    let age: Int
    let friends: [Friend]
}

struct Friend: Codable {
    let name: String
    let age: Int
}
```

To encode an instance of the `Person` struct to JSON, we can simply use the `JSONEncoder` class provided by Swift:

```swift
let person = Person(name: "John Doe", age: 30, friends: [
    Friend(name: "Alice", age: 28),
    Friend(name: "Bob", age: 32)
])

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

This will produce the following JSON output:

```json
{
  "name" : "John Doe",
  "age" : 30,
  "friends" : [
    {
      "name" : "Alice",
      "age" : 28
    },
    {
      "name" : "Bob",
      "age" : 32
    }
  ]
}
```

By using the `Codable` protocol and the `JSONEncoder` class, we can easily encode nested data structures in Swift.

#Swift #JSON