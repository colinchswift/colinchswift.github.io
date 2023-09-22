---
layout: post
title: "Implementing custom serialization and deserialization logic for complex data structures using Codable"
description: " "
date: 2023-09-22
tags: [Serialization, Deserialization]
comments: true
share: true
---

Serialization and deserialization are important concepts in software development, allowing us to convert complex data structures into a format that can be stored or transmitted, and then convert them back to their original form. In Swift, the `Codable` protocol provides a powerful and convenient way to achieve this.

## What is Codable?

Introduced in Swift 4, `Codable` is a protocol that combines the functionality of the `Encodable` and `Decodable` protocols. By conforming to `Codable`, you can easily convert your custom data structures to and from various formats such as JSON, property lists, and more.

## Basic Serialization and Deserialization with Codable

To get started with `Codable`, you need to define your data structure with properties that conform to `Codable`. For example, let's consider a simple `Person` struct with a `name` and an `age`.

```swift
struct Person: Codable {
    var name: String
    var age: Int
}
```

With this definition, you can now easily serialize an instance of `Person` to JSON using `JSONEncoder`.

```swift
let person = Person(name: "John Doe", age: 30)

do {
    let jsonData = try JSONEncoder().encode(person)
    let jsonString = String(data: jsonData, encoding: .utf8)
    print(jsonString)
} catch {
    print("Serialization error: \(error)")
}
```
The output will be:

```
{"name":"John Doe","age":30}
```

To deserialize the JSON back into a `Person` object, we can use `JSONDecoder`.

```swift
let json = """
{
    "name": "Jane Smith",
    "age": 25
}
"""

do {
    let jsonData = json.data(using: .utf8)
    let person = try JSONDecoder().decode(Person.self, from: jsonData!)
    print(person)
} catch {
    print("Deserialization error: \(error)")
}
```

## Customizing Serialization and Deserialization

Sometimes, you may need more control over the serialization and deserialization process, especially when dealing with more complex data structures. To customize the process, you can implement the `encode(to: Encoder)` and `init(from: Decoder)` methods provided by `Codable`.

For example, let's consider a `Book` struct that contains an array of `Author` objects.

```swift
struct Book: Codable {
    var title: String
    var authors: [Author]
}

struct Author: Codable {
    var name: String
    var age: Int
}
```

By default, encoding and decoding an array of `Author` objects in the `Book` struct will use the standard encoding and decoding behaviors of `Codable`. However, you can customize this behavior by implementing the methods mentioned earlier.

```swift
struct Book: Codable {
    var title: String
    var authors: [Author]
    
    enum CodingKeys: String, CodingKey {
        case title
        case authorsCustomKey = "custom_authors_key"
    }
    
    init(title: String, authors: [Author]) {
        self.title = title
        self.authors = authors
    }
    
    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(title, forKey: .title)
        try container.encode(authors, forKey: .authorsCustomKey)
    }
    
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        title = try container.decode(String.self, forKey: .title)
        authors = try container.decode([Author].self, forKey: .authorsCustomKey)
    }
}
```
In this example, we introduced a new `CodingKeys` enum to customize the property names in the encoded JSON. We defined `authorsCustomKey` as an alternative key for the `authors` property.

We then implemented the `encode(to:)` and `init(from:)` methods to manually encode and decode the `authors` array using the `authorsCustomKey`.

By providing this custom logic, we have more control over the encoding and decoding process while still utilizing the `Codable` protocol.

## Conclusion

Using `Codable` in Swift allows for seamless serialization and deserialization of custom data structures. Whether handling basic or complex data, Swift's `Codable` protocol simplifies the process by providing default encoding and decoding mechanisms. By customizing the serialization and deserialization logic using the `encode(to:)` and  `init(from:)` methods, you can further tailor the process to meet your specific requirements. #Serialization #Deserialization