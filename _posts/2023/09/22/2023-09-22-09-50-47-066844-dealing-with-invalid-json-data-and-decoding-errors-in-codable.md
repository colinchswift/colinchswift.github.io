---
layout: post
title: "Dealing with invalid JSON data and decoding errors in Codable"
description: " "
date: 2023-09-22
tags: [JSON]
comments: true
share: true
---

Introduction:
In the world of web development, dealing with JSON data and encoding/decoding it is a common task. However, things don't always go smoothly, and we may encounter scenarios where the JSON data is invalid or contains errors that can lead to decoding failures. In this article, we'll explore how to handle these situations effectively using Codable in Swift.

## Understanding Codable:
Swift's Codable protocol provides a convenient way to serialize and deserialize data between JSON and Swift types. It allows us to encode and decode our custom data structures, making working with JSON data much easier. However, Codable assumes that the JSON structure matches the structure defined in our Swift types. If the JSON data is invalid or contains errors, decoding can fail, throwing an error.

## Dealing with Invalid JSON Data:
When dealing with JSON that might be invalid, it's essential to wrap the decoding process inside a do-catch block to handle potential errors gracefully. Here's an example:

```swift
let jsonData = """
{
    "name": "John Doe",
    "age": 25,
    "email": "johndoe@example.com"
    // missing closing brace, making it invalid
""".data(using: .utf8)!

do {
    let decodedObject = try JSONDecoder().decode(MyModel.self, from: jsonData)
    // Process the decoded object
} catch {
    print("Error decoding JSON: \(error)")
    // Handle decoding failure
}
```

In the above code, if the JSON data is invalid (missing a closing brace in this case), the decoding process will throw an error, which we catch in the catch block. We can then handle the error gracefully, allowing our app to continue functioning rather than crashing.

## Handling Decoding Errors:

When decoding JSON, we may encounter errors even when the JSON structure is valid. For example, the data might miss a required field, the values might be of the wrong type, or the structure might have changed. In such cases, we can utilize custom coding keys and optional properties to handle decoding errors. Here's an example:

```swift
struct MyModel: Codable {
    let name: String
    let age: Int
    let email: String?

    enum CodingKeys: String, CodingKey {
        case name
        case age
        case email = "email_address"
    }

    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)

        self.name = try container.decode(String.self, forKey: .name)
        self.age = try container.decode(Int.self, forKey: .age)
        
        self.email = try container.decodeIfPresent(String.self, forKey: .email)
    }
}
```

In the above code snippet, we specify custom coding keys using the `enum CodingKeys`. By assigning a different key for `email`, we handle cases where the key name in the JSON structure differs from the property name in our Swift model.

Additionally, we use `decodeIfPresent` instead of `decode` for the optional `email` property. This allows us to handle situations where the JSON data doesn't contain the `email_address` key, avoiding decoding errors.

## Conclusion:
In summary, when working with Codable in Swift, it's crucial to handle invalid JSON data and decoding errors appropriately. By wrapping the decoding process in a do-catch block and utilizing custom coding keys and optional properties, we can gracefully handle decoding failures and ensure our app continues running smoothly.

#swift #JSON #Codable