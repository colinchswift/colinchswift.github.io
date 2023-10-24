---
layout: post
title: "Handling multiple JSON formats in Swift"
description: " "
date: 2023-10-24
tags: [JSON]
comments: true
share: true
---

When working with JSON in Swift, it is common to come across multiple JSON formats. These formats can vary based on the structure of the data or the naming conventions used. In this blog post, we will explore how to handle such scenarios and parse different JSON formats in Swift.

## Table of Contents
- [Introduction](#introduction)
- [Parsing JSON with different formats](#parsing-json-with-different-formats)
- [Using Codable protocol](#using-codable-protocol)
- [Working with custom JSON decodable](#working-with-custom-json-decodable)
- [Conclusion](#conclusion)

## Introduction

JSON (JavaScript Object Notation) is a popular data interchange format used for transmitting data between a server and a client. In Swift, the `Codable` protocol provides a convenient way to parse JSON by automatically transforming JSON data into Swift objects. However, handling multiple JSON formats can be challenging since the structure of the data may vary.

## Parsing JSON with different formats

To handle multiple JSON formats, we can leverage the power of conditional parsing and utilize different decoding strategies accordingly. Swift's `JSONDecoder` provides options like `keyDecodingStrategy` and `dataDecodingStrategy` that allow us to customize the parsing behavior.

## Using Codable protocol

In Swift, the `Codable` protocol provides a way to convert Swift structs and classes to and from JSON. By conforming to `Codable`, we can define the mapping between JSON keys and Swift properties using coding keys. This approach works well when the JSON format is consistent across different endpoints.

Here's an example of parsing JSON with a consistent format using `Codable`:

```swift
struct User: Codable {
    let id: Int
    let name: String
    let email: String
}

let jsonData = """
{
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com"
}
""".data(using: .utf8)!

let decoder = JSONDecoder()
let user = try decoder.decode(User.self, from: jsonData)
```

In this example, we define a `User` struct that conforms to `Codable`. We then use the `JSONDecoder` to decode the JSON data into a `User` object.

## Working with custom JSON decodable

In some cases, the JSON data may have inconsistent keys or nested structures that don't directly match our Swift types. In such scenarios, we can define custom decoding logic by implementing the `Decodable` protocol.

Here's an example of parsing JSON with a custom decoding logic:

```swift
struct Book: Decodable {
    let title: String
    let author: String

    enum CodingKeys: String, CodingKey {
        case title = "book_title"
        case author = "author_name"
    }

    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)

        title = try container.decode(String.self, forKey: .title)
        author = try container.decode(String.self, forKey: .author)
    }
}

let json = """
{
    "book_title": "The Catcher in the Rye",
    "author_name": "J.D. Salinger"
}
""".data(using: .utf8)!

let book = try JSONDecoder().decode(Book.self, from: json)
```

In this example, we define a `Book` struct that overrides the default coding keys using the `CodingKeys` enum. By providing custom keys, we can handle JSON data with different key names.

## Conclusion

Handling multiple JSON formats in Swift can be achieved by leveraging the power of `Codable` and custom decoding logic. By customizing the decoding strategies or implementing custom `Decodable` logic, we can parse JSON data that varies in structure or naming conventions.

It is important to consider the specific requirements of the JSON formats you are dealing with and choose the most appropriate approach for your use case.

#hashtags: #JSON #Swift