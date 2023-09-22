---
layout: post
title: "Using Codable to map and transform data between different APIs"
description: " "
date: 2023-09-22
tags: [Swift, Codable]
comments: true
share: true
---

In today's interconnected world, it is common for applications to consume data from multiple APIs. However, these APIs often return data in different formats or structures, making it challenging to handle and transform the data seamlessly. `Codable`, an advanced feature introduced in Swift 4, simplifies this process by providing a convenient way to map and transform data between different APIs.

## What is Codable?

`Codable` is a protocol introduced in Swift that allows types to be encoded and decoded to/from different external representations, such as JSON or property lists. By conforming to the `Codable` protocol, you can easily map data received from an API to Swift objects and vice versa.

## Mapping Data From API Responses

When working with APIs, it's common to receive responses in JSON format. With `Codable`, you can map this JSON data directly to Swift objects without writing additional parsing code.

Consider the following JSON response from an API:

```json
{
  "name": "John Doe",
  "age": 30,
  "email": "john.doe@example.com"
}
```

To map this JSON response to a Swift struct, simply define a struct that conforms to the `Codable` protocol and matches the structure of the JSON:

```swift
struct User: Codable {
    let name: String
    let age: Int
    let email: String
}
```

To decode the JSON response into a `User` object:

```swift
let jsonString = """
{
  "name": "John Doe",
  "age": 30,
  "email": "john.doe@example.com"
}
"""

if let jsonData = jsonString.data(using: .utf8) {
    let decoder = JSONDecoder()
    if let user = try? decoder.decode(User.self, from: jsonData) {
        print(user.name) // Output: John Doe
        print(user.age) // Output: 30
        print(user.email) // Output: john.doe@example.com
    }
}
```

## Transforming Data for API Requests

Similarly, `Codable` makes it easy to transform Swift objects into the required format when making API requests, such as converting Swift objects into JSON.

Consider the following Swift object:

```swift
struct CreatePostRequest: Codable {
    let title: String
    let body: String
}
```

To convert this Swift object into JSON for an API request:

```swift
let post = CreatePostRequest(title: "My Post", body: "This is the body of my post")

let encoder = JSONEncoder()
if let jsonData = try? encoder.encode(post),
   let jsonString = String(data: jsonData, encoding: .utf8) {
    print(jsonString)
}
```

The output will be:

```json
{
  "title": "My Post",
  "body": "This is the body of my post"
}
```

## Conclusion

`Codable` is a powerful and convenient feature in Swift that simplifies the mapping and transformation of data between different APIs. By leveraging the power of `Codable`, mapping data from API responses to Swift objects and transforming Swift objects into the required format for API requests becomes a seamless process. This results in clean, readable, and maintainable code, ultimately improving the development experience and productivity.

#Swift #Codable