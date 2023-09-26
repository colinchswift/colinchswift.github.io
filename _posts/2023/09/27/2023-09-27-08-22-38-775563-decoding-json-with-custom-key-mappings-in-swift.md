---
layout: post
title: "Decoding JSON with custom key mappings in Swift"
description: " "
date: 2023-09-27
tags: [json, codable]
comments: true
share: true
---

When working with JSON data in Swift, it's common to map JSON keys to corresponding properties in your Swift structs or classes. However, sometimes the JSON keys are not named exactly as the corresponding property in your code.

In such cases, you may need to provide custom key mappings during the decoding process. This can be achieved using the `CodingKeys` enum in Swift's `Codable` protocol.

Here's an example of decoding JSON with custom key mappings in Swift:

```swift
import Foundation

struct User: Codable {
    let name: String
    let email: String
    
    enum CodingKeys: String, CodingKey {
        case name = "full_name"
        case email = "user_email"
    }
}

let jsonString = """
{
    "full_name": "John Doe",
    "user_email": "johndoe@example.com"
}
"""

let jsonData = jsonString.data(using: .utf8)!

do {
    let decoder = JSONDecoder()
    let user = try decoder.decode(User.self, from: jsonData)
    
    print("Name: \(user.name)")
    print("Email: \(user.email)")
} catch {
    print("Error decoding JSON: \(error)")
}
```

In the above code, we define a `User` struct with `name` and `email` properties. The `CodingKeys` enum is used to provide custom key mappings - `"full_name"` maps to `name` and `"user_email"` maps to `email`.

When decoding the JSON using `JSONDecoder`, the mapping defined in `CodingKeys` is used to match the JSON keys with the corresponding properties in the `User` struct.

This allows you to decode JSON data even when the keys in the JSON don't match the property names in your code.

Using custom key mappings in your Swift Codable structs provides flexibility when working with JSON data. It allows you to handle different naming conventions and ensures seamless decoding of JSON into your Swift types.

#swift #json #codable