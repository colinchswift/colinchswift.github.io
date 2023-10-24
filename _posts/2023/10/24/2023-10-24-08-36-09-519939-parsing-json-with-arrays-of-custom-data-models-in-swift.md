---
layout: post
title: "Parsing JSON with arrays of custom data models in Swift"
description: " "
date: 2023-10-24
tags: [references]
comments: true
share: true
---

In many iOS applications, you may need to parse JSON responses from web APIs and convert them into custom data models. While parsing simple JSON objects is straightforward, parsing JSON arrays that contain custom data models can be a bit more complex. In this blog post, we'll explore how to parse JSON arrays of custom data models in Swift.

## Table of Contents
1. [Introduction](#introduction)
2. [Parsing JSON with Arrays](#parsing-json-with-arrays)
3. [Creating Custom Data Models](#creating-custom-data-models)
4. [Parsing JSON Arrays](#parsing-json-arrays)
5. [Conclusion](#conclusion)

## Introduction

JSON (JavaScript Object Notation) is a popular data interchange format that is widely used in web APIs. It represents data as key-value pairs and supports various data types, including strings, numbers, booleans, arrays, and objects.

In Swift, parsing JSON is made easier with the `Codable` protocol introduced in Swift 4. `Codable` provides a convenient way to convert between JSON data and Swift data types.

## Parsing JSON with Arrays

When an API response contains an array of objects, we need to define a data model that represents each object in the array. This data model should conform to the `Codable` protocol, allowing us to decode JSON into an instance of the data model.

## Creating Custom Data Models

To parse JSON arrays, we need to create custom data models that correspond to the structure of the JSON. Each property in the data model should have a corresponding key in the JSON object.

```swift
struct User: Codable {
    let id: Int
    let name: String
    let email: String
}
```

In the example above, we define a `User` struct with properties `id`, `name`, and `email`. The struct also conforms to the `Codable` protocol.

## Parsing JSON Arrays

To parse a JSON array of custom data models, we need to use a `JSONDecoder` instance to decode the JSON data. Here's an example of how to parse a JSON array of `User` objects:

```swift
let jsonString = """
[
    {"id": 1, "name": "John Doe", "email": "john.doe@example.com"},
    {"id": 2, "name": "Jane Smith", "email": "jane.smith@example.com"}
]
"""

let jsonData = jsonString.data(using: .utf8)!

do {
    let users = try JSONDecoder().decode([User].self, from: jsonData)
    for user in users {
        print(user.name)
    }
} catch {
    print("Error parsing JSON: \(error)")
}
```

In the code above, we define a JSON array as a string `jsonString` and convert it to `Data` using the `data(using:)` method. We then use a `JSONDecoder()` instance to decode the JSON data into an array of `User` objects.

If the JSON parsing is successful, we can access the parsed objects as an array and perform any necessary operations with them. Otherwise, the `catch` block will handle any decoding errors.

## Conclusion

Parsing JSON arrays of custom data models in Swift is made easy with the `Codable` protocol and the `JSONDecoder` class. By defining custom data models that conform to `Codable`, we can seamlessly convert JSON data into Swift objects, allowing us to work with API responses more efficiently.

#references #swift