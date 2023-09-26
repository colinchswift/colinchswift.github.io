---
layout: post
title: "Encoding and decoding JSON data using Codable"
description: " "
date: 2023-09-22
tags: [JSON]
comments: true
share: true
---

In many modern applications, working with JSON data is a common task. JSON (JavaScript Object Notation) is a lightweight data-interchange format that is easy to read and write. In this blog post, we will explore how to encode and decode JSON data using Swift's Codable protocol.

## What is Codable?

Codable is a protocol introduced in Swift 4 that combines the functionality of both encoding and decoding types. It provides a convenient way to convert your Swift models to and from JSON representation. By adopting Codable, you can easily encode your custom types into JSON and decode JSON data into your custom types.

## Encoding JSON data

To encode JSON data from your custom types, you'll need to make your types conform to the Codable protocol. The Codable protocol requires you to declare two methods:

### 1. Encoding

```swift
struct Person: Codable {
    let name: String
    let age: Int
}

let person = Person(name: "John Doe", age: 30)

let jsonEncoder = JSONEncoder()
if let jsonData = try? jsonEncoder.encode(person) {
    if let jsonString = String(data: jsonData, encoding: .utf8) {
        print(jsonString)
    }
}
```

In the above code, we define a `Person` struct that conforms to Codable. We create an instance of `Person` and then use a `JSONEncoder` object to encode the person object into a JSON representation. We then convert the JSON data into a string and print it.

The output JSON will be:

```json
{"name":"John Doe","age":30}
```

## Decoding JSON data

To decode JSON data into your custom types, you'll again need to make your types conform to the Codable protocol. The Codable protocol requires you to declare two methods:

### 2. Decoding

```swift
let jsonString = """
{
    "name": "John Doe",
    "age": 30
}
"""

let jsonDecoder = JSONDecoder()
if let jsonData = jsonString.data(using: .utf8) {
    if let person = try? jsonDecoder.decode(Person.self, from: jsonData) {
        print(person)
    }
}
```

In the above code, we declare a sample JSON string representing a `Person` object. We use a `JSONDecoder` object to decode the JSON data into a `Person` object. We then print the person object.

The output will be:

```plaintext
Person(name: "John Doe", age: 30)
```

## Summary

In this blog post, we learned how to encode and decode JSON data using Swift's Codable protocol. By conforming to the Codable protocol, you can easily interchange data between your custom types and JSON representation. This makes working with JSON data in Swift more convenient and less error-prone.

#Swift #JSON