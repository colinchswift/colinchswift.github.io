---
layout: post
title: "Transforming JSON values during decoding in Swift"
description: " "
date: 2023-09-27
tags: [JSONdecoding]
comments: true
share: true
---

When working with JSON data in Swift, we often need to decode the JSON into model objects using `Codable`. In some cases, we may need to transform JSON values during the decoding process. This can be useful when we want to convert the values to a different type or apply some custom logic.

In this blog post, we will explore how to transform JSON values during decoding in Swift using the `Codable` protocol and custom coding keys.

## 1. Transforming JSON values to a different type

To transform a JSON value to a different type during decoding, we can use the `transform` method of `JSONDecoder`. Let's consider an example where we have a JSON response that includes birth dates as timestamps, but we want our model object to store them as `Date` objects.

```swift
struct User: Codable {
    let name: String
    let birthDate: Date
}

let json = """
{
    "name": "John Doe",
    "birthDate": 946684800
}
""".data(using: .utf8)!

let decoder = JSONDecoder()
decoder.dateDecodingStrategy = .secondsSince1970

do {
    let user = try decoder.decode(User.self, from: json)
    print(user.name) // John Doe
    print(user.birthDate) // 2000-01-01 00:00:00 +0000
} catch {
    print(error)
}
```

In the above example, we set the `dateDecodingStrategy` of the JSONDecoder to `.secondsSince1970`, which treats the birth date value as a timestamp and converts it to a `Date` object during decoding.

## 2. Applying custom transformations during decoding

In some cases, we may need to apply custom transformations to JSON values during the decoding process. This can be achieved by implementing the `init(from decoder: Decoder)` method of `Codable`.

Let's consider an example where we have a JSON response that includes a boolean value represented as a string, but we want our model object to store it as a `Bool` value.

```swift
struct User: Codable {
    let name: String
    let isPremium: Bool
    
    private enum CodingKeys: String, CodingKey {
        case name
        case premiumStatus = "isPremium"
    }
    
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        
        name = try container.decode(String.self, forKey: .name)
        
        let premiumStatusString = try container.decode(String.self, forKey: .premiumStatus)
        isPremium = (premiumStatusString.lowercased() == "true")
    }
}

let json = """
{
    "name": "John Doe",
    "isPremium": "True"
}
""".data(using: .utf8)!

let decoder = JSONDecoder()

do {
    let user = try decoder.decode(User.self, from: json)
    print(user.name) // John Doe
    print(user.isPremium) // true
} catch {
    print(error)
}
```

In the above example, we implement the `init(from decoder: Decoder)` method and manually decode the `isPremium` key as a string. We then transform the string value to a boolean value based on our custom logic.

## Conclusion

Transforming JSON values during decoding in Swift allows us to convert values to different types or apply custom transformations. By leveraging `Codable` and custom coding keys, we can easily handle these scenarios and achieve a more flexible and efficient decoding process.

#codingswift #JSONdecoding