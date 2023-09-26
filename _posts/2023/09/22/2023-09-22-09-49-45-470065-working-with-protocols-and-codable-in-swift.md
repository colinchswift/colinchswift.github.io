---
layout: post
title: "Working with protocols and Codable in Swift"
description: " "
date: 2023-09-22
tags: [coding]
comments: true
share: true
---

Protocols and Codable are powerful features of the Swift programming language that allow for easy data encoding and decoding. By combining these two concepts, you can create flexible and efficient data models that can be easily serialized and deserialized.

## What is a Protocol?

In Swift, a protocol is a blueprint of methods, properties, and other requirements that can be adopted by a class, structure, or enumeration. It defines a set of rules or contract that an object must conform to in order to provide certain functionality.

```swift
protocol Vehicle {
    var name: String { get }
    func startEngine()
    func stopEngine()
}
```

Let's take the example of a `Vehicle` protocol that requires a `name` property and two methods, `startEngine()` and `stopEngine()`. Any class, structure, or enumeration that adopts this protocol must adhere to these requirements.

## What is Codable?

Codable is a protocol provided by Swift's `Foundation` framework that combines the functionality of `Encodable` and `Decodable` protocols. By conforming to the `Codable` protocol, you can automatically encode and decode your custom types to and from various representations, such as JSON or Property List.

```swift
struct Car: Codable {
    var name: String
    var model: String
    var year: Int
}
```

In this example, we have a `Car` structure that conforms to the `Codable` protocol. This means that we can easily convert a `Car` instance into a JSON representation and vice versa.

## Combining Protocols and Codable

When working with Codable, you can also make use of protocols to define common functionality across different types that need to be encoded or decoded.

```swift
protocol Vehicle: Codable {
    var name: String { get }
    func startEngine()
    func stopEngine()
}
```

By adding the `Codable` protocol to the `Vehicle` protocol, we can now have multiple types that conform to both protocols. This allows us to encode and decode instances of these types without explicitly handling each type.

```swift
struct Car: Vehicle {
    var name: String
    var model: String
    var year: Int

    func startEngine() {
        print("Engine started")
    }

    func stopEngine() {
        print("Engine stopped")
    }
}
```

In this example, the `Car` structure conforms to both the `Vehicle` and `Codable` protocols. We can now encode and decode instances of the `Car` structure using the `JSONEncoder` and `JSONDecoder` classes provided by Swift's `Foundation` framework.

## Conclusion

Combining protocols and Codable in Swift allows for powerful and flexible data encoding and decoding. By defining protocols that conform to Codable, you can easily encode and decode multiple types without duplicating code. This approach helps to keep your codebase clean, maintainable, and efficient.

#swift #coding