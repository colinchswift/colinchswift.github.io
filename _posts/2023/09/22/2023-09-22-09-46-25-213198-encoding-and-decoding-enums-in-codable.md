---
layout: post
title: "Encoding and decoding enums in Codable"
description: " "
date: 2023-09-22
tags: [Codable]
comments: true
share: true
---

Enums provide a way to define a group of related values in Swift. When working with JSON or other data formats, you may come across situations where you need to encode and decode these enums using Codable. In this blog post, we will explore how to encode and decode enums in Codable in Swift.

## Encoding Enums

To encode an enum using Codable, you need to conform to the `Encodable` protocol and implement the `encode(to:)` method. Let's consider an example where we have an enum `Color` representing different colors.

```swift
enum Color: String, Codable {
    case red
    case blue
    case green
}
```

To encode this enum, we can use the `rawValue` property of the enum, which is of type `String` in this case. The `encode(to:)` method will convert the enum value to its raw value and encode it into the desired format. Here's an example of how to encode a `Color` enum using JSONEncoder:

```swift
let color = Color.blue

do {
   let jsonData = try JSONEncoder().encode(color)
   let jsonString = String(data: jsonData, encoding: .utf8)
   print(jsonString) // Output: "blue"
} catch {
   print("Encoding failed: \(error)")
}
```

In this example, we encode the `Color.blue` enum value into a JSON string, which will contain the raw value `"blue"`.

## Decoding Enums

To decode an enum using Codable, you need to conform to the `Decodable` protocol and implement the `init(from:)` initializer. Let's continue with the `Color` enum example and demonstrate how to decode a color from a JSON string.

```swift
let jsonString = """
{
    "color": "red"
}
"""

struct ColorResponse: Decodable {
    let color: Color
}

do {
    let jsonData = jsonString.data(using: .utf8)!
    let colorResponse = try JSONDecoder().decode(ColorResponse.self, from: jsonData)
    let color = colorResponse.color
    print(color) // Output: red
} catch {
    print("Decoding failed: \(error)")
}
```

In this example, we define a `ColorResponse` struct that contains a `color` property of type `Color`. By using the raw value of the enum, the JSONDecoder can match the raw value `"red"` to the `Color.red` enum value.

## Conclusion

Encoding and decoding enums in Codable can be easily achieved by leveraging the `rawValue` property and implementing the `encode(to:)` method for encoding and the `init(from:)` initializer for decoding. This allows you to seamlessly work with enums in your Codable models, enabling robust data serialization and deserialization. By understanding these techniques, you can harness the power of Swift's Codable to work with enums effectively. #Swift #Codable