---
layout: post
title: "Conditional conformance for Codable types in Swift"
description: " "
date: 2023-09-27
tags: [Swift, Codable]
comments: true
share: true
---

Swift's Codable protocol provides a convenient way to encode and decode data from and to various formats such as JSON. However, there are scenarios where you may encounter types that contain properties of generic types, and you want them to be Codable as well. This is where conditional conformance comes into play.

## What is Conditional Conformance?

In Swift, conditional conformance allows you to extend a generic type's conformance to a protocol based on certain conditions. This means that if specific conditions are met, the generic type will automatically conform to the protocol.

## Using Conditional Conformance with Codable

Let's say you have a Car struct that contains an array of items of generic type T. You wish to make the Car struct Codable, but Codable itself is not directly applicable to generic types. To achieve this, you can leverage conditional conformance:

```swift
struct Car<T>: Codable where T: Codable {
    var items: [T]
    
    // Other properties and methods...
}
```

In this example, the Car struct is declared with a generic type `T` which must conform to the Codable protocol. By adding the `where T: Codable` constraint, we are telling Swift that the Car struct is only Codable if its generic type T is Codable as well.

## Encoded and Decoding Generic Types

Once you have made the Car struct Codable, you can encode and decode instances to and from JSON, just like any other Codable type. Here's an example of encoding and decoding Car instances:

```swift
let car = Car(items: ["Engine", "Tire", "Exhaust"])
    
let encoder = JSONEncoder()
let encodedData = try encoder.encode(car)
    
let decoder = JSONDecoder()
let decodedCar = try decoder.decode(Car.self, from: encodedData)

print(decodedCar.items) // Output: ["Engine", "Tire", "Exhaust"]
```
In this example, we create a car instance with an array of items and encode it using a JSONEncoder. Then, we decode the encoded data using a JSONDecoder to retrieve the original instance.

## Conclusion

Conditional conformance allows you to extend existing protocols like Codable to generic types. This enables you to encode and decode instances of types that contain generic properties. By making use of conditional conformance, you can enhance the flexibility and reuse of your code when working with Codable types in Swift.

#Swift #Codable