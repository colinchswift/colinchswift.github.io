---
layout: post
title: "Decoding JSON into Swift structs"
description: " "
date: 2023-09-27
tags: [Swift, JSON]
comments: true
share: true
---

With the advent of RESTful APIs, handling JSON data has become a fundamental skill for mobile app development. Swift, being a powerful and modern programming language, provides simple and efficient ways to decode JSON into custom structs. In this blog post, we will explore how to decode JSON into Swift structs using the Codable protocol.

## The Codable Protocol

Introduced in Swift 4, the Codable protocol provides a convenient way to encode and decode Swift types to and from external representations, such as JSON.

To decode JSON into a Swift struct, you need to ensure that the struct conforms to the Codable protocol by implementing the necessary `init(from decoder: Decoder)` initializer.

## Example

Let's assume we have a JSON response from an API that looks like this:

```json
{
  "name": "John Doe",
  "age": 30,
  "email": "johndoe@example.com"
}
```

To decode this JSON into a Swift struct, we need to define a struct that represents the JSON structure:

```swift
struct User: Codable {
    let name: String
    let age: Int
    let email: String
}
```

To decode the JSON, we can use the `JSONDecoder` class provided by the Foundation framework:

```swift
// Assuming jsonData is the data received from the API
do {
    let decoder = JSONDecoder()
    let user = try decoder.decode(User.self, from: jsonData)
    print(user.name) // Output: John Doe
    print(user.age) // Output: 30
    print(user.email) // Output: johndoe@example.com
} catch {
    print("Error decoding JSON: \(error)")
}
```

By calling `decode(_:from:)` on the `JSONDecoder` instance, we pass the type (`User.self`) we want to decode the JSON into and the data to be decoded.

If no encoding or decoding errors occur, we can access the decoded values from the struct instance.

## Conclusion

Swift's Codable protocol provides a clean and straightforward way to decode JSON into Swift structs. By following a few simple steps, you can effortlessly map JSON data to custom data types. This not only simplifies your code but also helps you in handling API responses more effectively.

Try experimenting with different JSON structures and see how Codable simplifies the decoding process for you!

#Swift #JSON