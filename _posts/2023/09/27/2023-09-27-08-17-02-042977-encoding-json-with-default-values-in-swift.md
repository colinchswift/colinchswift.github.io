---
layout: post
title: "Encoding JSON with default values in Swift"
description: " "
date: 2023-09-27
tags: []
comments: true
share: true
---

JSON (JavaScript Object Notation) is a popular data interchange format used in various programming languages, including Swift. When encoding Swift structs or classes to JSON, there might be cases where certain properties have default values that you don't want to include in the JSON representation.

In Swift, you can use the `Codable` protocol to easily encode and decode Swift types to JSON. By default, all properties of a `Codable` type are included when encoding to JSON. However, you can customize this behavior and exclude properties with default values from being encoded.

To exclude properties with default values from the JSON representation, you can define a custom `CodingKeys` enum for your `Codable` type and implement the `init(from:)` initializer.

Here's an example that demonstrates how to encode a `Person` struct to JSON with default values:

```swift
struct Person: Codable {
    let name: String
    let age: Int
    let hasSubscription: Bool

    // Define a custom CodingKeys enum
    private enum CodingKeys: String, CodingKey {
        case name
        case age
        case hasSubscription
    }

    // Implement init(from:) to exclude properties with default values
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        name = try container.decode(String.self, forKey: .name)
        age = try container.decode(Int.self, forKey: .age)
        hasSubscription = try container.decodeIfPresent(Bool.self, forKey: .hasSubscription) ?? false
    }
    
    // Implement encode(to:) to encode the struct to JSON
    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(name, forKey: .name)
        try container.encode(age, forKey: .age)

        // Exclude property from encoding if it has the default value
        if hasSubscription {
            try container.encode(hasSubscription, forKey: .hasSubscription)
        }
    }
}

// Usage

let person = Person(name: "John Doe", age: 30, hasSubscription: false)

do {
    let encoder = JSONEncoder()
    encoder.outputFormatting = .prettyPrinted
    let jsonData = try encoder.encode(person)
    if let jsonString = String(data: jsonData, encoding: .utf8) {
        print(jsonString)
    }
} catch {
    print("Error encoding person to JSON: \(error)")
}
```

In the example above, the `hasSubscription` property has a default value of `false`. It is excluded from the JSON representation if its value matches the default value.