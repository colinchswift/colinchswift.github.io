---
layout: post
title: "Safely handling Codable decoding errors and invalid data in production apps"
description: " "
date: 2023-09-22
tags: []
comments: true
share: true
---

The Codable protocol in Swift provides a convenient way to encode and decode data between Swift types and external representations such as JSON or Property List. However, when working with real-world data, it's important to handle decoding errors and invalid data in a safe and robust manner. In this blog post, we'll explore some best practices for handling decoding errors and dealing with invalid data when using Codable in production apps.

## 1. Use optional properties or custom initializers

When defining Codable types, it's a good practice to make properties optional if they can contain invalid or missing data. This allows the decoder to skip over these properties if they cannot be decoded correctly. For example:

```swift
struct UserProfile: Codable {
    let username: String
    let age: Int?
}

let jsonData = """
{
    "username": "johnappleseed"
}
""".data(using: .utf8)!

do {
    let userProfile = try JSONDecoder().decode(UserProfile.self, from: jsonData)
    // Handle decoded data

} catch {
    // Handle decoding error
}
```

In this example, the `age` property is made optional in case it's missing from the JSON data or cannot be decoded. This ensures that the decoding process doesn't fail when encountering invalid data.

Alternatively, you can also define custom initializers for your Codable types to perform additional validation and handle any decoding errors more granularly. This can be useful when dealing with complex data structures or when you need more control over the decoding process.

## 2. Provide default values for missing properties

If certain properties in your Codable types are required and cannot be optional, you can provide default values when they are missing from the decoded data. This can be done by implementing the `init(from decoder: Decoder)` initializer and manually assigning default values to these properties. For example:

```swift
struct UserProfile: Codable {
    let username: String
    let age: Int
    
    enum CodingKeys: String, CodingKey {
        case username
        case age
    }
    
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        
        username = try container.decode(String.self, forKey: .username)
        age = try container.decodeIfPresent(Int.self, forKey: .age) ?? 0
    }
}

let jsonData = """
{
    "username": "johnappleseed"
}
""".data(using: .utf8)!

do {
    let userProfile = try JSONDecoder().decode(UserProfile.self, from: jsonData)
    // Handle decoded data

} catch {
    // Handle decoding error
}
```

In this example, if the `age` property is missing from the JSON data, it will be assigned a default value of 0. This ensures that the `UserProfile` object is still valid even if the age is not provided.

## 3. Use a flexible error-handling strategy

When decoding Codable types, it's important to have a robust error-handling strategy in place. Instead of relying solely on the generic `Error` type, consider using specific error types or custom enums to provide more context about the decoding failure. This allows you to handle different types of errors in a more specific and meaningful way.

```swift
enum UserProfileDecodingError: Error {
    case invalidData
    case dataMismatch
    case missingRequiredField(String)
}

```

In this example, we define a custom enum `UserProfileDecodingError` to represent different decoding errors that can occur when decoding a `UserProfile`. By doing this, we can catch specific errors and handle them accordingly, providing a better user experience and more meaningful error messages.

## Conclusion

When working with Codable in production apps, it's crucial to handle decoding errors and invalid data in a safe and robust manner. By using optional properties, providing default values, and using a flexible error-handling strategy, you can ensure that your app gracefully handles different scenarios and provides a good user experience.