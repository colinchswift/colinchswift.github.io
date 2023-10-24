---
layout: post
title: "Handling different JSON formats in Swift"
description: " "
date: 2023-10-24
tags: []
comments: true
share: true
---

JSON (JavaScript Object Notation) is a popular data interchange format used in web services and APIs. It provides a simple and lightweight way to represent structured data. However, JSON can come in different formats and structures, making it necessary to handle these variations when parsing and working with JSON data in Swift.

In this article, we will explore some common techniques for handling different JSON formats in Swift and discuss how to handle scenarios where the structure or keys in the JSON response can vary.

## 1. Using Codable

Swift's Codable protocol provides a convenient way to serialize and deserialize JSON data. By adopting the Codable protocol, you can define a mapping between your Swift data model and the JSON representation automatically.

```swift
struct Person: Codable {
    var name: String
    var age: Int
}

let json = """
{
    "name": "John Doe",
    "age": 25
}
"""

let jsonData = json.data(using: .utf8)!
let decoder = JSONDecoder()

do {
    let person = try decoder.decode(Person.self, from: jsonData)
    print(person.name) // Output: John Doe
    print(person.age) // Output: 25
} catch {
    print("Error decoding JSON: \(error)")
}
```

By using Codable, Swift automatically maps the properties of the `Person` struct to the corresponding keys in the JSON data.

## 2. Handling optional keys

In some cases, the JSON response may contain optional keys. To handle this situation, you can define optional properties in your data model.

```swift
struct Person: Codable {
    var name: String
    var age: Int
    var address: String?
}

let json = """
{
    "name": "John Doe",
    "age": 25
}
"""

let jsonData = json.data(using: .utf8)!
let decoder = JSONDecoder()

do {
    let person = try decoder.decode(Person.self, from: jsonData)
    print(person.address) // Output: nil (optional property not present in the JSON)
} catch {
    print("Error decoding JSON: \(error)")
}
```

Here, the `address` property is defined as optional, allowing it to handle scenarios where the JSON does not include that key.

## 3. Handling different key names

Sometimes, the key names in the JSON response may not match the property names in your Swift data model. In such cases, you can use the `CodingKeys` enum to specify the mapping between the JSON keys and your model's properties.

```swift
struct Person: Codable {
    var name: String
    var age: Int

    private enum CodingKeys: String, CodingKey {
        case name = "full_name"
        case age
    }
}

let json = """
{
    "full_name": "John Doe",
    "age": 25
}
"""

let jsonData = json.data(using: .utf8)!
let decoder = JSONDecoder()

do {
    let person = try decoder.decode(Person.self, from: jsonData)
    print(person.name) // Output: John Doe
} catch {
    print("Error decoding JSON: \(error)")
}
```

By specifying the `CodingKeys` enum, you can map the JSON key "full_name" to the `name` property in your Swift model.

## Conclusion

Handling different JSON formats is a common requirement when working with web services and APIs. Swift provides powerful tools like Codable to simplify the parsing and handling of JSON data. By using Codable, handling different JSON formats becomes easier and more maintainable.