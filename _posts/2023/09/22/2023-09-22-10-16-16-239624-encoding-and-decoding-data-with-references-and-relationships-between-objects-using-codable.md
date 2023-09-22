---
layout: post
title: "Encoding and decoding data with references and relationships between objects using Codable"
description: " "
date: 2023-09-22
tags: [DevTips, SwiftCoding]
comments: true
share: true
---

In modern app development, processing data is a common task. Whether it is fetching data from a network or persisting it locally, you often need to convert the data from one representation to another. Swift's Codable protocol provides a convenient way to encode and decode data to and from different formats, such as JSON or Property Lists.

## What is Codable?

Codable is a protocol introduced in Swift 4 that allows you to encode and decode data in Swift. It combines the functionality of the Encodable and Decodable protocols into one, making it easy to work with different data formats. By adopting the Codable protocol, you can serialize and deserialize Swift objects to and from various formats without writing extensive parsing code.

## Implementing Codable

To make your custom objects Codable, you need to conform to the Codable protocol and provide implementations for the required methods. Let's say we have two classes: `Person` and `Company`, and a `Company` can have multiple `Person` objects. Here's an example of how you can implement encoding and decoding for these classes:

```swift
struct Person: Codable {
    var name: String
    var age: Int
}

struct Company: Codable {
    var name: String
    var employees: [Person]
}
```

In the above code snippet, both `Person` and `Company` adopt the Codable protocol by conforming to it. By doing this, you can now use the `JSONEncoder` and `JSONDecoder` classes to convert instances of these objects to and from JSON.

## Encoding Data

To encode an object to JSON, you can use the `JSONEncoder` class. Here's how you can encode a `Company` object:

```swift
let company = Company(name: "Apple Inc.", employees: [
    Person(name: "Tom", age: 30),
    Person(name: "Lisa", age: 28)
])

do {
    let encoder = JSONEncoder()
    let jsonData = try encoder.encode(company)
    
    if let jsonString = String(data: jsonData, encoding: .utf8) {
        print(jsonString)
    }
} catch {
    print("Encoding failed: \(error)")
}
```

The JSONEncoder converts the `Company` object to a Data object, which can be converted to a string representation.

## Decoding Data

To decode JSON data into an object, you can use the `JSONDecoder` class. Here's how you can decode a `Company` object from the previously encoded JSON:

```swift
let jsonString = """
{
    "name": "Apple Inc.",
    "employees": [
        {
            "name": "Tom",
            "age": 30
        },
        {
            "name": "Lisa",
            "age": 28
        }
    ]
}
"""

guard let jsonData = jsonString.data(using: .utf8) else {
    fatalError("Invalid JSON string")
}

do {
    let decoder = JSONDecoder()
    let company = try decoder.decode(Company.self, from: jsonData)
    print(company)
} catch {
    print("Decoding failed: \(error)")
}
```

The JSONDecoder converts the JSON data into a `Company` object using the `decode(_:from:)` method. If the decoding is successful, you can access the properties of the decoded object.

# Conclusion

The Codable protocol in Swift makes it easy to encode and decode data to and from various formats, such as JSON. By adopting the Codable protocol and implementing the required methods, you can have efficient and maintainable data processing in your Swift applications. Whether you're working with simple objects or complex relationships between them, Codable provides a straightforward way to handle data encoding and decoding.

#DevTips #SwiftCoding