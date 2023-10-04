---
layout: post
title: "Working with different JSON data formats in Swift"
description: " "
date: 2023-09-27
tags: [JSON]
comments: true
share: true
---

JSON (JavaScript Object Notation) is a popular data interchange format used in web development and API communication. In Swift, you can easily work with JSON data using the `Codable` protocol and JSONEncoder/JSONDecoder classes. However, when dealing with different JSON data formats, such as snake_case or camelCase, some additional steps are required to handle the data correctly. 

## Understanding Snake Case and Camel Case

In JSON, the keys can be formatted as snake_case or camelCase. Snake case uses underscores between words, while camel case uses uppercase for the first letter of each word except the first word.

For example, consider the following JSON objects:

```swift
// Snake case formatted JSON
{
  "first_name": "John",
  "last_name": "Doe"
}

// Camel case formatted JSON
{
  "firstName": "John",
  "lastName": "Doe"
}
```

## Convert Snake Case JSON to Swift Struct (or Class)

To work with snake case formatted JSON in Swift, you need to specify the `keyDecodingStrategy` property of your `JSONDecoder` instance. By default, Swift expects camel case format, so you need to tell it to decode snake case keys instead.

You can achieve this by creating a custom decoder:

```swift
let decoder = JSONDecoder()
decoder.keyDecodingStrategy = .convertFromSnakeCase

let jsonData = // JSON data to be decoded
let myModel = try decoder.decode(MyModel.self, from: jsonData)
```

Here, `MyModel` is your Swift struct or class conforming to the `Decodable` protocol that represents the JSON data.

## Convert Swift Struct (or Class) to Snake Case JSON

To convert your Swift struct or class into snake case formatted JSON, you can set the `keyEncodingStrategy` property of your `JSONEncoder` instance:

```swift
let encoder = JSONEncoder()
encoder.keyEncodingStrategy = .convertToSnakeCase

let myModel = // Your Swift struct or class instance
let jsonData = try encoder.encode(myModel)
```

This will encode your Swift object with snake case keys.

## Conclusion

When working with different JSON data formats in Swift, such as snake case or camel case, it's important to configure your JSON encoder and decoder accordingly. By setting the appropriate key encoding and decoding strategies, you can ensure proper interaction with JSON data in different formats.

#Swift #JSON