---
layout: post
title: "Encoding and decoding nested JSON objects in Swift"
description: " "
date: 2023-09-27
tags: [Swift, JSON]
comments: true
share: true
---
title: Encoding and Decoding Nested JSON Objects in Swift
tags: #Swift #JSON
---

In Swift, working with JSON is a common task when dealing with web APIs or parsing data. JSON (JavaScript Object Notation) is a lightweight data-interchange format that is easy for humans to read and write, and easy for machines to parse and generate.

While encoding and decoding JSON objects in Swift is relatively straightforward, it becomes slightly more complex when dealing with nested JSON structures. In this blog post, we will explore how to encode and decode nested JSON objects in Swift.

## Swift's Codable Protocol

Swift provides a powerful protocol called `Codable` that allows you to easily encode and decode custom data types. By adopting the `Codable` protocol, you can encode your custom data structures into JSON and decode JSON data into your custom data types.

Let's consider a scenario where we have a nested JSON object representing a user and their address:

```swift
struct Address: Codable {
    var street: String
    var city: String
    var postalCode: String
}

struct User: Codable {
    var name: String
    var age: Int
    var address: Address
}
```

In the above code snippet, we have defined two structs `Address` and `User`, both adopting the `Codable` protocol. The `User` struct contains an `Address` object.

## Encoding Nested JSON Objects

To encode our nested JSON object, we can use the `JSONEncoder` class provided by Swift. Here's an example of encoding a `User` object:

```swift
let address = Address(street: "123 Main Street", city: "Example City", postalCode: "12345")
let user = User(name: "John Doe", age: 25, address: address)

let encoder = JSONEncoder()
encoder.outputFormatting = .prettyPrinted

do {
    let jsonData = try encoder.encode(user)
    if let jsonString = String(data: jsonData, encoding: .utf8) {
        print(jsonString)
    }
} catch {
    print("Error encoding user object:", error)
}
```

In the above code, we create an instance of `JSONEncoder` and set its output formatting to `.prettyPrinted` to make the JSON output more readable. We then encode the `User` object into JSON data and convert it to a string representation.

## Decoding Nested JSON Objects

To decode the nested JSON object back into our custom data types, we can use the `JSONDecoder` class. Here's an example of decoding a JSON string into a `User` object:

```swift
let jsonString = """
{
    "name": "John Doe",
    "age": 25,
    "address": {
        "street": "123 Main Street",
        "city": "Example City",
        "postalCode": "12345"
    }
}
"""

let decoder = JSONDecoder()

do {
    let jsonData = jsonString.data(using: .utf8)!
    let user = try decoder.decode(User.self, from: jsonData)
    print(user)
} catch {
    print("Error decoding JSON:", error)
}
```

In the above code snippet, we create an instance of `JSONDecoder` and then decode the JSON string into a `User` object using the `decode(_:from:)` method.

## Conclusion

Working with nested JSON objects in Swift is made easy with the `Codable` protocol and Swift's built-in JSON encoding and decoding capabilities. By adopting `Codable` and using the `JSONEncoder` and `JSONDecoder` classes, you can effortlessly encode and decode complex JSON structures in your Swift applications.

Remember to **encode** your data before sending it to an API endpoint and **decode** the JSON response you receive from the API. By doing so, you can ensure seamless communication between your Swift application and the server-side API.

#Swift #JSON