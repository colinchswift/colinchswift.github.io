---
layout: post
title: "Transforming and manipulating JSON data during encoding and decoding with Codable"
description: " "
date: 2023-09-22
tags: [JSONManipulation]
comments: true
share: true
---

In Swift, Codable is a powerful protocol that allows for easy encoding and decoding of JSON data. However, it can also be used to transform and manipulate JSON data during the encoding and decoding process. In this blog post, we will explore how to take advantage of Codable to transform and manipulate JSON data to meet our specific requirements.

## 1. Transforming JSON Data during Encoding

### Custom Encoding Strategy

Sometimes, we need to modify the JSON data before it is encoded. For example, let's say we have a `Person` struct with a `birthdate` property of type `Date` and we want to encode it as a formatted string instead of a raw `Date` object. We can achieve this by providing a custom encoding strategy using the `JSONEncoder`'s `dateEncodingStrategy` property.

```swift
struct Person: Codable {
    let name: String
    let birthdate: Date

    enum CodingKeys: String, CodingKey {
        case name
        case birthdate = "date_of_birth"
    }

    struct DateOfBirthFormatter {
        static let formatter: DateFormatter = {
            let formatter = DateFormatter()
            formatter.dateFormat = "yyyy-MM-dd"
            return formatter
        }()
    }

    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(name, forKey: .name)

        let dateString = DateOfBirthFormatter.formatter.string(from: birthdate)
        try container.encode(dateString, forKey: .birthdate)
    }
}

let person = Person(name: "John Doe", birthdate: Date())

let encoder = JSONEncoder()
encoder.outputFormatting = .prettyPrinted
encoder.dateEncodingStrategy = .custom {
    (date, encoder) in
    var container = encoder.singleValueContainer()
    try container.encode(DateOfBirthFormatter.formatter.string(from: date))
}

do {
    let jsonData = try encoder.encode(person)
    if let jsonString = String(data: jsonData, encoding: .utf8) {
        print(jsonString)
    }
} catch {
    print(error)
}
```

In the above example, we have overridden the `encode(to:)` method of the `Person` struct to provide custom encoding logic for the `birthdate` property. We format the `birthdate` as a string using a specific date format and encode it using the `dateEncodingStrategy`.

## 2. Manipulating JSON Data during Decoding

### Custom Decoding Strategy

Similarly to encoding, we can also manipulate the JSON data during the decoding process. Let's consider the scenario where we have an API response that returns a JSON object with a `price` field represented as a string, and we want to convert it to a `Double` for further calculations.

```swift
struct Product: Codable {
    let name: String
    let price: Double

    enum CodingKeys: String, CodingKey {
        case name
        case price
    }

    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        name = try container.decode(String.self, forKey: .name)

        let priceString = try container.decode(String.self, forKey: .price)
        price = Double(priceString) ?? 0.0
    }
}

let json = """
{
    "name": "iPhone",
    "price": "999.99"
}
"""

let data = json.data(using: .utf8)!

let decoder = JSONDecoder()

do {
    let product = try decoder.decode(Product.self, from: data)
    print(product)
} catch {
    print(error)
}
```

In the above example, we have created a custom init method `init(from:)` in the `Product` struct to provide custom decoding logic for the `price` property. We decode the `price` as a string and convert it to a `Double` using `Double(priceString) ?? 0.0`. If the conversion fails, we set a default value of 0.0.

## Conclusion

By leveraging the power of Codable in Swift, we can easily transform and manipulate JSON data during the encoding and decoding process. Whether it's modifying the data before encoding or converting it to a different format during decoding, Codable provides the flexibility to handle such transformations efficiently. By understanding and utilizing custom encoding and decoding strategies, we can tailor the JSON data to our specific requirements.

#Swift #JSONManipulation