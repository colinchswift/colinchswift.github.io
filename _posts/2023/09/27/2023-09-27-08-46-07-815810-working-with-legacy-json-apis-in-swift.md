---
layout: post
title: "Working with legacy JSON APIs in Swift"
description: " "
date: 2023-09-27
tags: [JSON]
comments: true
share: true
---

In Swift, working with legacy JSON APIs can be challenging as they may not conform to modern JSON encoding and decoding standards. These APIs often lack properly formatted keys, have inconsistent naming conventions, or contain nested structures that require additional handling. In this blog post, we'll explore some strategies and techniques to make working with legacy JSON APIs easier in Swift.

## 1. Analyzing the JSON Structure

Before diving into parsing the JSON response, it's essential to understand its structure. To analyze the JSON structure, you can use online JSON viewers or JSON parsing tools. Understanding the structure will help you identify any irregularities or inconsistencies that you need to handle while decoding.

```swift
let jsonData = """
{
    "id": 1,
    "name": "John",
    "email_address": "john@example.com",
    "phone-number": "1234567890",
    "address": {
        "street": "123 Main St",
        "city": "New York",
        "country": "USA"
    }
}
""".data(using: .utf8)

struct User: Codable {
    let id: Int
    let name: String
    let emailAddress: String
    let phoneNumber: String
    let address: Address

    enum CodingKeys: String, CodingKey {
        case id
        case name
        case emailAddress = "email_address"
        case phoneNumber = "phone-number"
        case address
    }
}

struct Address: Codable {
    let street: String
    let city: String
    let country: String
}

do {
    let user = try JSONDecoder().decode(User.self, from: jsonData)
    print(user)
} catch {
    print("Error decoding JSON: \(error)")
}
```

In the above code snippet, we have a sample JSON response with inconsistent key names. By using the `CodingKeys` enum, we can map the JSON keys to our Swift struct's property names to ensure proper decoding.

## 2. Handling Nested Structures

Legacy JSON APIs sometimes have nested structures that require extra handling. By extending the `User` struct, we can add a custom initializer that handles the nested `address` structure.

```swift
extension User {
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        id = try container.decode(Int.self, forKey: .id)
        name = try container.decode(String.self, forKey: .name)
        emailAddress = try container.decode(String.self, forKey: .emailAddress)
        phoneNumber = try container.decode(String.self, forKey: .phoneNumber)
        
        // Handle nested address structure
        let addressContainer = try container.nestedContainer(keyedBy: Address.CodingKeys.self, forKey: .address)
        street = try addressContainer.decode(String.self, forKey: .street)
        city = try addressContainer.decode(String.self, forKey: .city)
        country = try addressContainer.decode(String.self, forKey: .country)
    }
}
```

By implementing a custom initializer, we can extract the nested structure from the JSON response and decode it into the respective properties of the `User` struct.

## Conclusion

Working with legacy JSON APIs in Swift requires careful analysis of the JSON structure and proper handling of irregularities. By using key mappings and custom initializers, we can ensure accurate decoding of the JSON response. Understanding the structure and adopting these techniques will make working with legacy JSON APIs more manageable and less error-prone.

#JSON #API