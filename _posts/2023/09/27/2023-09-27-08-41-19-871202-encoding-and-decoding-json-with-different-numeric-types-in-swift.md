---
layout: post
title: "Encoding and decoding JSON with different numeric types in Swift"
description: " "
date: 2023-09-27
tags: [JSON]
comments: true
share: true
---

JSON (JavaScript Object Notation) is a popular data interchange format used for transmitting data between a server and a client. Swift provides built-in support for encoding and decoding JSON using the `Codable` protocol. However, when working with numeric values in JSON, you may encounter situations where Swift's default behavior doesn't align with the desired representation. In this blog post, we will explore how to handle different numeric types when encoding and decoding JSON in Swift.

## Understanding Numeric Types in JSON

JSON natively supports only two numeric types: numbers (including integers and floating-point numbers) and strings. When encoding JSON, numeric values are typically stored as either numbers or strings, depending on the desired behavior. Similarly, when decoding JSON, Swift needs to convert JSON numbers or strings to appropriate Swift numeric types.

## Encoding Numeric Types in Swift

To encode numeric types in Swift, we can use the `JSONEncoder` class provided by the `Foundation` framework. By default, `JSONEncoder` encodes numeric values as JSON numbers. 

```swift
struct Book: Codable {
    let id: Int
    let title: String
    let price: Decimal
}

let book = Book(id: 123, title: "The Great Gatsby", price: Decimal(29.99))

do {
    let encoder = JSONEncoder()
    let data = try encoder.encode(book)
    let jsonString = String(data: data, encoding: .utf8)!
    print(jsonString)
} catch {
    print("Encoding failed: \(error)")
}
```

In the example above, we have a `Book` struct that includes an `id` (Int), `title` (String), and `price` (Decimal). When encoding the `Book` instance using `JSONEncoder`, the decimal value of `price` is automatically encoded as a JSON number.

## Decoding Numeric Types in Swift

Decoding JSON with different numeric representations can be more challenging. Swift's default behavior is to throw a `DecodingError` if it encounters unexpected or incompatible types while decoding JSON.

To handle different numeric types, we need to implement a custom initializer that overrides the default decoder behavior. In this case, we can utilize the `JSONDecoder` class provided by the `Foundation` framework.

```swift
struct Book: Codable {
    let id: Int
    let title: String
    let price: Decimal

    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        id = try container.decode(Int.self, forKey: .id)
        title = try container.decode(String.self, forKey: .title)

        if let priceString = try? container.decode(String.self, forKey: .price) {
            if let priceValue = Decimal(string: priceString) {
                price = priceValue
            } else {
                throw DecodingError.dataCorruptedError(forKey: .price,
                                                       in: container,
                                                       debugDescription: "Invalid decimal value")
            }
        } else {
            price = try container.decode(Decimal.self, forKey: .price)
        }
    }
}

let jsonString = """
    {
        "id": 123,
        "title": "The Great Gatsby",
        "price": "29.99"
    }
"""

let jsonData = jsonString.data(using: .utf8)!

do {
    let decoder = JSONDecoder()
    let book = try decoder.decode(Book.self, from: jsonData)
    print(book) // Book(id: 123, title: "The Great Gatsby", price: 29.99)
} catch {
    print("Decoding failed: \(error)")
}
```

In this example, we have an updated `Book` struct with a custom initializer. Inside the initializer, we first attempt to decode the `price` value as a string using `container.decode(String.self, forKey: .price)`. If successful, we then convert the string representation to a `Decimal` value using `Decimal(string:)`. If any errors occur during the decoding process, we can throw a `DecodingError` with a custom message to handle the failure gracefully.

## Conclusion

When working with different numeric types in JSON, it's important to understand how to properly encode and decode values. Swift's `Codable` protocol, along with the `JSONEncoder` and `JSONDecoder` classes from the `Foundation` framework, provide powerful tools for working with JSON data. By implementing custom initializer methods, we can handle various numeric representations and ensure smooth encoding and decoding processes.

#JSON #Swift