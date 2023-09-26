---
layout: post
title: "Encoding and decoding Codable objects in different formats (CSV, YAML, etc.)"
description: " "
date: 2023-09-22
tags: [Codable]
comments: true
share: true
---

In today's digital world, data comes in various formats. As a developer, you may encounter scenarios where you need to encode and decode Swift objects in different formats like CSV, YAML, or even XML. Thankfully, Apple's Codable protocol makes it straightforward to serialize objects to different formats and deserialize them back into Swift objects.

## What is Codable?

Codable is a powerful Swift protocol introduced in iOS 10 and macOS 10.12 that allows you to encode and decode Swift objects to and from external representations like JSON, Property Lists, and more. By adopting Codable in your Swift class or struct, you declare that it can be encoded and decoded using the supported formats.

## Encoding Codable Objects

To encode a Codable object, you'll typically start by instantiating an instance of an encoder for the desired format. Let's walk through an example of encoding a Swift object into CSV.

```swift
import Foundation

struct Person: Codable {
    let name: String
    let age: Int
    let profession: String
}

let person = Person(name: "John Doe", age: 30, profession: "Software Developer")

let csvEncoder = CSVEncoder()
csvEncoder.delimiter = ","

do {
    let csvData = try csvEncoder.encode(person)
    let csvString = String(data: csvData, encoding: .utf8)
    print(csvString ?? "")
} catch {
    print("Failed to encode person object: \(error)")
}
```

In this example, we define a `Person` struct that conforms to the Codable protocol. We then create an instance of `Person` and an instance of `CSVEncoder`. The `CSVEncoder` allows us to specify custom options like the delimiter used to separate fields in the CSV file. Finally, we use the encoder's `encode()` method to encode the `person` object into a CSV representation. The resulting data can be converted to a string if needed.

## Decoding Codable Objects

Decoding works the same way as encoding. You need to instantiate a decoder for the desired format and use its `decode()` method to create an instance of your Swift object.

Here's an example of decoding a CSV string into a Swift object:

```swift
import Foundation

let csvString = "\"John Doe\",30,\"Software Developer\""
let csvData = csvString.data(using: .utf8)!

let csvDecoder = CSVDecoder()
csvDecoder.delimiter = ","

do {
    let person = try csvDecoder.decode(Person.self, from: csvData)
    print(person)
} catch {
    print("Failed to decode CSV string: \(error)")
}
```

In this example, we have a CSV string representation of a `Person` object. We convert it to `Data` using `data(using: .utf8)` and then create an instance of `CSVDecoder`. We set the delimiter to match the one used in the CSV string. Finally, we use the decoder's `decode()` method, passing in the type of `Person`, to decode the CSV data into a `Person` object.

## Conclusion

Thanks to Codable, Swift provides a unified way to encode and decode objects in different formats. Whether you're working with CSV, YAML, or any other format, the Codable protocol allows you to easily serialize and deserialize your Swift objects without much effort. By embracing Codable, you can write cleaner, more maintainable code that seamlessly works with various data formats.

#Swift #Codable #Encoding #Decoding