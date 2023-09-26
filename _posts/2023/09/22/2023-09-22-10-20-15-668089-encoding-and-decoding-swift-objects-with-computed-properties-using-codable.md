---
layout: post
title: "Encoding and decoding Swift objects with computed properties using Codable"
description: " "
date: 2023-09-22
tags: [Codable]
comments: true
share: true
---

In Swift, the `Codable` protocol provides a convenient way to encode and decode objects to and from various data formats such as JSON and property lists. However, when working with objects that contain computed properties, some additional steps are required to ensure proper encoding and decoding. In this blog post, we will explore how to encode and decode Swift objects with computed properties using `Codable`.

## Understanding Computed Properties in Swift

In Swift, computed properties are properties that do not store a value directly but instead provide a getter and optionally a setter to retrieve and set a value indirectly. They are declared using the `var` keyword and are followed by a block of code enclosed in curly braces `{}`.

Here's an example of a computed property that calculates the area of a rectangle based on its width and height:

```swift
struct Rectangle {
    var width: Double
    var height: Double
    
    var area: Double {
        return width * height
    }
}
```

## Encoding Objects with Computed Properties

When encoding Swift objects that contain computed properties, we need to provide a custom implementation of the `encode(to:)` method from the `Encodable` protocol. In this custom implementation, we manually encode all the necessary properties, including the computed properties.

Here's an example of encoding our `Rectangle` struct using `JSONEncoder`:

```swift
struct Rectangle: Codable {
    var width: Double
    var height: Double
    
    var area: Double {
        return width * height
    }
    
    enum CodingKeys: String, CodingKey {
        case width
        case height
        case area
    }
    
    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(width, forKey: .width)
        try container.encode(height, forKey: .height)
        try container.encode(area, forKey: .area)
    }
}

let rectangle = Rectangle(width: 10, height: 5)
let encoder = JSONEncoder()
if let encodedData = try? encoder.encode(rectangle) {
    let jsonString = String(data: encodedData, encoding: .utf8)
    print(jsonString)
}
```

## Decoding Objects with Computed Properties

Similarly, when decoding Swift objects with computed properties, we need to provide a custom implementation of the `init(from:)` initializer from the `Decodable` protocol. In this custom initializer, we manually decode all the required properties, including the computed properties.

Here's an example of decoding our `Rectangle` struct using `JSONDecoder`:

```swift
struct Rectangle: Codable {
    var width: Double
    var height: Double
    
    var area: Double {
        return width * height
    }
    
    enum CodingKeys: String, CodingKey {
        case width
        case height
    }
    
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        width = try container.decode(Double.self, forKey: .width)
        height = try container.decode(Double.self, forKey: .height)
    }
}

let jsonString = """
{
    "width": 10,
    "height": 5
}
"""

let decoder = JSONDecoder()
if let jsonData = jsonString.data(using: .utf8) {
    if let decodedRectangle = try? decoder.decode(Rectangle.self, from: jsonData) {
        print(decodedRectangle.area)
    }
}
```

## Conclusion

By providing custom implementations of the `encode(to:)` method and `init(from:)` initializer, we can successfully encode and decode Swift objects with computed properties using the `Codable` protocol. This enables us to work with complex data models and seamlessly convert them to and from different data formats. 

#Swift #Codable