---
layout: post
title: "Converting JSON strings to Swift classes"
description: " "
date: 2023-09-27
tags: [JSON]
comments: true
share: true
---

JSON (JavaScript Object Notation) is a widely used data interchange format. It provides a way to represent data in a structured and readable format, making it popular for sending and receiving data over the web. In Swift, it is common to convert JSON strings into Swift classes to handle and manipulate the data more effectively.

There are several libraries available in Swift that can help with this conversion process, such as Codable, ObjectMapper, or SwiftyJSON. In this blog post, we will focus on using the Codable protocol, introduced in Swift 4, to convert JSON strings to Swift classes.

## The Codable Protocol

The `Codable` protocol provides a convenient way to serialize and deserialize data between Swift classes and external representations, such as JSON. By adopting the `Codable` protocol, a Swift class can automatically convert its properties to and from JSON without writing lengthy conversion code manually.

To make a Swift class `Codable`, it needs to conform to the `Codable` protocol. The protocol is composed of two other protocols: `Encodable` and `Decodable`. `Encodable` allows for encoding (converting the class properties to JSON), while `Decodable` allows for decoding (converting JSON to class properties).

Here's an example of a Swift class that conforms to the `Codable` protocol:

```swift
struct User: Codable {
    let id: Int
    let username: String
    let email: String
}
```

## Converting JSON to Swift classes

To convert a JSON string to a Swift class, we first need to decode the JSON using the `JSONDecoder` class provided by the `Foundation` framework. The `JSONDecoder` class takes care of the decoding process and maps the JSON data to the corresponding Swift class properties.

Here's an example of converting a JSON string to a Swift class using the `Codable` protocol:

```swift
let jsonString = """
    {
        "id": 1,
        "username": "john_doe",
        "email": "john.doe@example.com"
    }
"""

if let jsonData = jsonString.data(using: .utf8) {
    do {
        let user = try JSONDecoder().decode(User.self, from: jsonData)
        print(user.id)
        print(user.username)
        print(user.email)
    } catch {
        print("Error decoding JSON: \(error.localizedDescription)")
    }
}
```

In this example, we create a `JSONDecoder` instance and call the `decode` method, specifying the type of the Swift class (`User`) we want to convert the JSON to. If the decoding is successful, we can access the properties of the `User` instance to retrieve the data.

## Converting Swift classes to JSON

Converting Swift classes to JSON is equally straightforward. We can use the `JSONEncoder` class to encode the Swift class properties as JSON.

Here's an example of converting a Swift class to JSON:

```swift
let user = User(id: 1, username: "john_doe", email: "john.doe@example.com")

do {
    let jsonData = try JSONEncoder().encode(user)
    if let jsonString = String(data: jsonData, encoding: .utf8) {
        print(jsonString)
    }
} catch {
    print("Error encoding to JSON: \(error.localizedDescription)")
}
```

In this example, we create a `JSONEncoder` instance and call the `encode` method, passing the Swift class instance (`user`) we want to convert to JSON. If the encoding is successful, we can obtain the JSON string representation by converting the `Data` object back to a `String`.

## Conclusion

Converting JSON strings to Swift classes and vice versa is made easy with the `Codable` protocol and the `JSONDecoder` and `JSONEncoder` classes. By leveraging the power of Swift's native support for JSON serialization, developers can efficiently work with JSON data in their Swift applications.

#Swift #JSON