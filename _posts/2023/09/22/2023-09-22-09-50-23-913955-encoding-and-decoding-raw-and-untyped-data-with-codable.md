---
layout: post
title: "Encoding and decoding raw and untyped data with Codable"
description: " "
date: 2023-09-22
tags: [tech, programming]
comments: true
share: true
---

In modern software development, handling different types of data is a common task. Often, you need to encode or decode raw and untyped data to a structured format that can be easily processed. This is where Codable, a powerful feature introduced in Swift 4, comes to the rescue.

**What is Codable?**
Codable is a protocol provided by Swift that combines the functionalities of the Encodable and Decodable protocols. It allows you to seamlessly convert between **objects** and **data** without having to write complex serialization and deserialization logic.

**Encoding Data**
To encode raw and untyped data, you need to create a **struct** or **class** that conforms to the Encodable protocol. Let's say you have a `Person` struct with name and age properties:

```swift
struct Person: Encodable {
    var name: String
    var age: Int
}
```

To encode an instance of this struct, you simply use the `JSONEncoder` class as follows:

```swift
let person = Person(name: "John Doe", age: 30)

do {
    let jsonData = try JSONEncoder().encode(person)
    // jsonData is a Data object containing the encoded JSON representation
    // You can now send or store this data as needed
} catch {
    print("Encoding failed: \(error.localizedDescription)")
}
```

**Decoding Data**
To decode raw and untyped data, you need to create a struct or class that conforms to the Decodable protocol. Let's continue with the `Person` struct from the previous example:

```swift
struct Person: Decodable {
    var name: String
    var age: Int
}
```

To decode JSON data into an instance of this struct, you can use the `JSONDecoder` class:

```swift
let json = """
{
    "name": "John Doe",
    "age": 30
}
"""

do {
    let jsonData = Data(json.utf8)
    let person = try JSONDecoder().decode(Person.self, from: jsonData)
    // 'person' is an instance of the Person struct with the name and age properties populated
} catch {
    print("Decoding failed: \(error.localizedDescription)")
}
```

**Conclusion**
Using Codable, you no longer have to write boilerplate code to encode and decode raw data. It simplifies the process and makes working with different data formats much easier. Whether you are working with JSON, XML, or other data types, Codable provides a unified approach to handle encoding and decoding. So go ahead and leverage this powerful feature in Swift for a more efficient data processing workflow.

*#swift #codable #json #dataencoding #datadecoding*