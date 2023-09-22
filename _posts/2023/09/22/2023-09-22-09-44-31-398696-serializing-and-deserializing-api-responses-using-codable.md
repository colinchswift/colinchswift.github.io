---
layout: post
title: "Serializing and deserializing API responses using Codable"
description: " "
date: 2023-09-22
tags: [Serialization]
comments: true
share: true
---

In modern application development, interacting with APIs is a common task. One important aspect of working with APIs is serializing and deserializing the data exchanged between the client and the API endpoint. This allows us to convert the JSON or XML responses received from the API into meaningful objects in our code. 

## Codable Protocol

Introduced in Swift 4, the `Codable` protocol provides a simple way to convert between Swift types and external representations such as JSON or XML. By adopting the `Codable` protocol, we can easily encode Swift objects into data for sending requests or decode data received from API responses into Swift objects.

To make a custom Swift type codable, we need to conform to the `Codable` protocol and provide implementations for the `Encodable` and `Decodable` protocols. Here is an example:

```swift
struct User: Codable {
    let id: Int
    let name: String
    let email: String
}
```

The `User` struct adopts the `Codable` protocol by conforming to the `Encodable` and `Decodable` protocols. This allows instances of `User` to be easily serialized and deserialized.

## Encoding Objects to JSON

To encode an object to JSON data, we can use the `JSONEncoder` class. Here's an example of encoding a `User` object:

```swift
let user = User(id: 1, name: "John Doe", email: "john@example.com")

do {
    let jsonEncoder = JSONEncoder()
    let jsonData = try jsonEncoder.encode(user)
    
    if let jsonString = String(data: jsonData, encoding: .utf8) {
        print(jsonString)
        // Output: {"id":1,"name":"John Doe","email":"john@example.com"}
    }
} catch let error {
    print("Encoding error: \(error.localizedDescription)")
}
```

The `JSONEncoder` converts the `User` object into JSON data. We can then convert the data into a JSON string using the `String` initializer.

## Decoding JSON to Objects

To decode JSON data into objects, we can use the `JSONDecoder` class. Here's an example of decoding a JSON string into a `User` object:

```swift
let jsonString = "{\"id\":1,\"name\":\"John Doe\",\"email\":\"john@example.com\"}"

do {
    let jsonDecoder = JSONDecoder()
    let jsonData = jsonString.data(using: .utf8)!
    let user = try jsonDecoder.decode(User.self, from: jsonData)
    
    print(user)
    // Output: User(id: 1, name: "John Doe", email: "john@example.com")
} catch let error {
    print("Decoding error: \(error.localizedDescription)")
}
```

The `JSONDecoder` converts the JSON string into JSON data. We can then decode the data into a `User` object using the `decode(_:from:)` method.

## Conclusion

The `Codable` protocol provides a powerful and convenient way to serialize and deserialize data when working with APIs. By adopting `Codable`, we can easily convert Swift objects to JSON and vice versa, removing the need for manual parsing and serialization/deserialization code. This simplifies working with APIs and improves the maintainability of our code.

#API #Serialization