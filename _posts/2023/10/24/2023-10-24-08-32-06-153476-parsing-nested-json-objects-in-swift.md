---
layout: post
title: "Parsing nested JSON objects in Swift"
description: " "
date: 2023-10-24
tags: [JSON]
comments: true
share: true
---

When working with JSON in Swift, it is common to encounter nested JSON objects. Parsing these nested objects can be a bit more challenging than parsing a flat JSON structure. In this blog post, we will look at how to parse nested JSON objects in Swift using the `Codable` protocol and provide a step-by-step guide for handling nested JSON structures.

## Table of Contents
- [Introduction](#introduction)
- [Understanding the JSON Structure](#understanding-the-json-structure)
- [Creating Structs for Nested JSON Objects](#creating-structs-for-nested-json-objects)
- [Implementing Codable](#implementing-codable)
- [Parsing Nested JSON Objects](#parsing-nested-json-objects)
- [Conclusion](#conclusion)

## Introduction
JSON (JavaScript Object Notation) is a popular data format used for APIs and data exchange between platforms. In Swift, we can use the `Codable` protocol to parse and serialize JSON data. However, when dealing with nested JSON objects, we need to define the appropriate structures to correctly parse the nested data.

## Understanding the JSON Structure
Before we start parsing nested JSON objects, it is important to understand the structure of the JSON data. Let's consider the following example:

```swift
{
  "name": "John",
  "age": 30,
  "address": {
    "street": "123 Main St",
    "city": "New York",
    "country": "USA"
  }
}
```

In this example, we have a nested JSON object called `address` within the main JSON object. We need to define structures in Swift to represent both the main object and the nested object.

## Creating Structs for Nested JSON Objects
To parse the nested JSON object, we need to create nested structs that mirror the structure of the JSON data. In our example, we would define the following structs:

```swift
struct Address: Codable {
  let street: String
  let city: String
  let country: String
}

struct Person: Codable {
  let name: String
  let age: Int
  let address: Address
}
```

Here, we define a `Person` struct with properties `name`, `age`, and `address`. The `address` property is of type `Address`, which represents the nested JSON object.

## Implementing Codable
To parse the JSON data, our structs need to conform to the `Codable` protocol. The `Codable` protocol provides default implementations for encoding and decoding values to and from JSON. In our example, both `Address` and `Person` structs need to conform to `Codable`:

```swift
struct Address: Codable {
  let street: String
  let city: String
  let country: String
}

struct Person: Codable {
  let name: String
  let age: Int
  let address: Address
}
```

## Parsing Nested JSON Objects
We can now parse the nested JSON objects using the `JSONDecoder` class provided by Swift's `Foundation` framework. Here's an example of how to parse the JSON data and retrieve the nested objects:

```swift
let json = """
{
  "name": "John",
  "age": 30,
  "address": {
    "street": "123 Main St",
    "city": "New York",
    "country": "USA"
  }
}
"""

let jsonData = json.data(using: .utf8)!
do {
  let person = try JSONDecoder().decode(Person.self, from: jsonData)
  print(person.name) // Output: John
  print(person.address.street) // Output: 123 Main St
} catch {
  print("Error parsing JSON: \(error)")
}
```

In this code snippet, we create a `Person` instance by decoding the JSON data into our `Person` struct. We can then access the properties of the `Person` object, including the nested `address` object.

## Conclusion
Parsing nested JSON objects in Swift is made easy by leveraging the power of the `Codable` protocol. By defining appropriate nested structs and conforming to `Codable`, we can easily parse complex JSON structures. Understanding the JSON structure and creating the corresponding Swift structs is essential for successful parsing.

By following the steps outlined in this blog post, you should now have a good understanding of how to parse nested JSON objects in Swift. Happy coding!

**#swift #JSON**

References:
- [Apple Developer Documentation - Encoding and Decoding Custom Types](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types)
- [Hacking with Swift - Codable](https://www.hackingwithswift.com/sixty/5/5/codable)