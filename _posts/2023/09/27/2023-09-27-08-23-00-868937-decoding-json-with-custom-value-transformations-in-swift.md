---
layout: post
title: "Decoding JSON with custom value transformations in Swift"
description: " "
date: 2023-09-27
tags: [json, decoding]
comments: true
share: true
---

When working with JSON data in Swift, we often need to map the JSON keys to different property names or transform the values before using them in our app. JSON decoding can be easily accomplished using the Codable protocol in Swift, but sometimes we need more control over how the values are transformed.

In this blog post, we will explore how to perform custom value transformations during JSON decoding in Swift. We will demonstrate this using the `JSONDecoder` and a custom implementation of the `Decodable` protocol.

## The Problem

Let's imagine we have the following JSON data representing a user:

```json
{
   "name": "John Smith",
   "date_of_birth": "1990-01-01",
   "email_address": "john@example.com",
   "phone_number": "+1 123-456-7890"
}
```

We want to decode this JSON into a `User` struct with the following properties:

```swift
struct User {
   var name: String
   var dateOfBirth: Date
   var emailAddress: String
   var phoneNumber: String
}
```

However, notice that the JSON keys don't match the property names exactly, and the `date_of_birth` field needs to be transformed into a `Date` object.

## Custom Value Transformations

To handle the custom value transformations, we can implement our own decoding logic by conforming to the `Decodable` protocol.

Let's start by creating an extension for our `User` struct that adopts the `Decodable` protocol. Inside this extension, we can define a private enum to hold the keys for the JSON properties.

```swift
extension User: Decodable {
    private enum CodingKeys: String, CodingKey {
        case name
        case dateOfBirth = "date_of_birth"
        case emailAddress = "email_address"
        case phoneNumber = "phone_number"
    }
}
```

Next, we need to implement the `init(from decoder: Decoder)` initializer, which is required by the `Decodable` protocol. Inside this initializer, we can decode each property using the keys we defined earlier and apply any necessary transformations.

```swift
extension User {
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        
        name = try container.decode(String.self, forKey: .name)
        emailAddress = try container.decode(String.self, forKey: .emailAddress)
        phoneNumber = try container.decode(String.self, forKey: .phoneNumber)
        
        let dateOfBirthString = try container.decode(String.self, forKey: .dateOfBirth)
        dateOfBirth = DateFormatter.iso8601.date(from: dateOfBirthString) ?? Date()
    }
}
```

In this example, we use a custom `CodingKey` enum to map the JSON keys to the appropriate properties. We then manually decode each property, applying transformations when necessary.

## Conclusion

By adopting the `Decodable` protocol and implementing our own custom logic for value transformations, we can easily handle complex JSON decoding scenarios in Swift.

Custom value transformations can be useful when working with non-standard JSON or when we need to conform to specific data formats.

Remember to import the necessary frameworks, such as Foundation, to access the required types and classes used during the decoding process.

#swift #json #decoding