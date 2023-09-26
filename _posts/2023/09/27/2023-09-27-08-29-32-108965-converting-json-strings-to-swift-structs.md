---
layout: post
title: "Converting JSON strings to Swift structs"
description: " "
date: 2023-09-27
tags: [JSON, Codable]
comments: true
share: true
---

## Introduction

Parsing JSON data is a common task when working with APIs or handling data in a Swift application. Swift offers a convenient way to decode JSON data into custom structs using its built-in `Codable` protocol. In this blog post, we'll explore how to convert JSON strings into Swift structs using the `Codable` protocol.

## JSON to Swift Structs

To convert a JSON string to Swift structs, we'll follow these steps:

### 1. Create a Swift struct

First, let's create a Swift struct that represents the structure of our JSON data. Each property in the struct should match the key names in the JSON data.

```swift
struct User: Codable {
    var id: Int
    var name: String
    var email: String
}
```

### 2. Parse JSON string

Next, we need to parse the JSON string into a Swift object using the `JSONDecoder` class. The decoder automatically maps the JSON keys to the struct properties.

```swift
let jsonString = """
{
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com"
}
"""

let data = Data(jsonString.utf8)

do {
    let user = try JSONDecoder().decode(User.self, from: data)
    // Access the properties of `user`
    print(user.name)
} catch {
    print("Error decoding JSON: \(error)")
}
```

### 3. Handling nested JSON objects

If the JSON data contains nested objects or arrays, we can define additional structs to represent those nested structures. The nested structs should also conform to the `Codable` protocol.

```swift
struct Post: Codable {
    var id: Int
    var title: String
}

struct User: Codable {
    var id: Int
    var name: String
    var email: String
    var posts: [Post]
}
```

By defining the necessary structs, the `JSONDecoder` will automatically handle the decoding of nested structures.

## Conclusion

In this blog post, we've learned how to convert JSON strings into Swift structs using the `Codable` protocol. This provides a convenient way to parse and work with JSON data in Swift applications. With the `Codable` protocol, we can easily decode JSON data and map it to our custom struct types. Happy coding!

#swift #JSON #Codable