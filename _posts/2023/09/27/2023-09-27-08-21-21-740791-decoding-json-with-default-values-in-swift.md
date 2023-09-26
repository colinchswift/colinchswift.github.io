---
layout: post
title: "Decoding JSON with default values in Swift"
description: " "
date: 2023-09-27
tags: [Swift]
comments: true
share: true
---

JSON decoding is a common task in any iOS app that interacts with a backend server. In Swift, we can use the powerful `Codable` protocol to easily decode JSON data into Swift types. However, there are scenarios where we want to provide default values for certain JSON keys that may be missing. In this blog post, we will explore how to decode JSON with default values in Swift.

## Adding default values to Codable structs

To decode JSON with default values, we can modify our `Codable` structs to include the `init(from:)` initializer. This allows us to customize the decoding process and provide default values for missing keys.

Let's say we have the following JSON structure representing a user:

```swift
{
    "name": "John Doe",
    "age": 25
}
```

And we have a corresponding `User` struct in Swift:

```swift
struct User: Codable {
    let name: String
    let age: Int
}
```

To provide a default value for the `age` key, we can extend the `User` struct and implement the `init(from:)` initializer:

```swift
extension User {
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        
        name = try container.decode(String.self, forKey: .name)
        age = try container.decodeIfPresent(Int.self, forKey: .age) ?? 0
    }
}
```

In the above implementation, we use the `decodeIfPresent(_:forKey:)` method to decode the `age` key. If the key is present, we decode its value as an `Int`. If it's missing, we provide the default value of `0`.

## Testing JSON decoding with default values

To test our JSON decoding with default values, we can use the `JSONDecoder` class provided by Swift:

```swift
let json = """
{
    "name": "John Doe"
}
""".data(using: .utf8)!

let decoder = JSONDecoder()
let user = try decoder.decode(User.self, from: json)

print(user.name) // Output: John Doe
print(user.age) // Output: 0
```

In the above example, we only have the "name" key in the JSON data. When decoding, the `age` key is missing, so it's assigned the default value of `0`.

## Conclusion

Decoding JSON with default values is a useful technique when dealing with optional keys or when we want to provide fallback values for missing keys. By customizing the `init(from:)` initializer of our `Codable` structs, we can easily handle default values during the JSON decoding process in Swift.

#iOS #Swift