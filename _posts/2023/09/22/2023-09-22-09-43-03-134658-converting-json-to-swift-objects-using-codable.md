---
layout: post
title: "Converting JSON to Swift objects using Codable"
description: " "
date: 2023-09-22
tags: [JSON]
comments: true
share: true
---

In Swift, **Codable** is a powerful protocol that allows for easy conversion between JSON data and native Swift objects. With just a few lines of code, you can decode JSON into Swift objects and encode Swift objects into JSON.

To get started, make sure your Swift objects conform to the Codable protocol. This protocol combines two other protocols: `Decodable` and `Encodable`. `Decodable` is used for decoding JSON into Swift objects, while `Encodable` is used for encoding Swift objects into JSON.

Here's an example of a Swift struct representing a Person:

```swift
struct Person: Codable {
    let name: String
    let age: Int
}
```

To convert JSON data into Swift objects, use the `JSONDecoder` class. You can simply call the `decode(_:from:)` method, which takes in the type of the Swift object you want to decode and the JSON data.

```swift
let jsonData = """
    {
        "name": "John Doe",
        "age": 25
    }
    """.data(using: .utf8)!

do {
    let person = try JSONDecoder().decode(Person.self, from: jsonData)
    print(person.name) // Output: John Doe
    print(person.age) // Output: 25
} catch {
    print("Error decoding JSON: \(error)")
}
```

In this example, we create a JSON string and convert it into `Data` using the `data(using:)` method. Then, we use the `JSONDecoder` to decode the JSON data into a `Person` object. If the decoding is successful, we can access the properties of the `Person` object.

Conversely, to convert Swift objects into JSON data, use the `JSONEncoder` class. Similar to decoding, you can call the `encode(_:to:)` method.

```swift
let person = Person(name: "John Doe", age: 25)

do {
    let jsonData = try JSONEncoder().encode(person)
    let jsonString = String(data: jsonData, encoding: .utf8)
    print(jsonString) // Output: {"name":"John Doe","age":25}
} catch {
    print("Error encoding to JSON: \(error)")
}
```

In this example, we create a `Person` object and encode it into JSON data using the `JSONEncoder`. We then convert the JSON data to a JSON string and print it out.

By following the `Codable` protocol and using the `JSONDecoder` and `JSONEncoder` classes, converting between JSON and Swift objects becomes simple and efficient.

#Swift #JSON #Codable