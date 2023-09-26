---
layout: post
title: "Encoding JSON with custom key mappings in Swift"
description: " "
date: 2023-09-27
tags: [Swift, JSON]
comments: true
share: true
---

In Swift, encoding JSON data is a common task when working with APIs or sending data to servers. Sometimes, you may encounter situations where the key names in your Swift code may not match the key names in the expected JSON format. In such cases, you can use custom key mappings to map your Swift property names to the corresponding JSON keys.

Let's say we have a `User` struct with properties `firstName` and `lastName`, but the API expects the keys to be `first_name` and `last_name` respectively. We can encode the JSON data with custom key mappings using the `CodingKey` protocol in Swift.

Here's an example of how to encode JSON data with custom key mappings in Swift:

1. Define the `User` struct with the desired property names:
```swift
struct User: Codable {
    let firstName: String
    let lastName: String
}
```
2. Define a nested `CodingKeys` enum inside the `User` struct to represent the desired JSON keys:
```swift
struct User: Codable {
    let firstName: String
    let lastName: String

    private enum CodingKeys: String, CodingKey {
        case firstName = "first_name"
        case lastName = "last_name"
    }
}
```
3. Implement the `encode(to:)` function to encode the properties using the custom key mappings:
```swift
struct User: Codable {
    let firstName: String
    let lastName: String

    private enum CodingKeys: String, CodingKey {
        case firstName = "first_name"
        case lastName = "last_name"
    }

    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(firstName, forKey: .firstName)
        try container.encode(lastName, forKey: .lastName)
    }
}
```
4. Use the `JSONEncoder` to encode a `User` instance into JSON data:
```swift
let user = User(firstName: "John", lastName: "Doe")
let encoder = JSONEncoder()
let jsonData = try encoder.encode(user)
```
The resulting JSON data will have the keys `first_name` and `last_name` as specified in the `CodingKeys` enum.

By using custom key mappings, you can easily encode Swift structs to JSON with different key names without having to change the property names within the struct. This approach provides flexibility when working with APIs that require specific key naming conventions.

#Swift #JSON #CodingKey