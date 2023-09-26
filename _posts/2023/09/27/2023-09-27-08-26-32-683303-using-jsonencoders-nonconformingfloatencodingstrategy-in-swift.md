---
layout: post
title: "Using JSONEncoder's nonConformingFloatEncodingStrategy in Swift"
description: " "
date: 2023-09-27
tags: [Swift, JSONEncoder]
comments: true
share: true
---

In Swift, `JSONEncoder` is a powerful class that allows you to encode Swift types into JSON data. One of the challenges when encoding floating-point values is that JSON only supports the finite set of valid floating-point numbers defined by the IEEE 754 standard. However, Swift has a wider range of possible floating-point values, including NaN and infinity.

To handle encoding non-conforming floating-point values in Swift, you can use the `nonConformingFloatEncodingStrategy` property of `JSONEncoder`. This property allows you to specify how you want to encode floating-point values that do not conform to the JSON standards.

The `nonConformingFloatEncodingStrategy` property takes an enum value of type `JSONEncoder.NonConformingFloatEncodingStrategy`, which has three cases:

1. `throw`: This strategy throws an error when attempting to encode non-conforming floating-point values. You can catch the error and handle it as needed.
2. `convertToString`: This strategy converts non-conforming floating-point values to a string before encoding them. This ensures that the resulting JSON is valid and can be reliably decoded.
3. `convertToInfinity`: This strategy converts non-conforming floating-point values to infinity before encoding them. This is useful if you want to represent non-conforming values as infinity in the resulting JSON.

## Example Usage

Here's an example that demonstrates how to use the `nonConformingFloatEncodingStrategy` property with `JSONEncoder`:

```swift
struct Person: Codable {
    var name: String
    var height: Double
    var weight: Double
}

let person = Person(name: "John Doe", height: Double.infinity, weight: Double.NaN)

let encoder = JSONEncoder()
encoder.nonConformingFloatEncodingStrategy = .convertToString

do {
    let jsonData = try encoder.encode(person)
    let jsonString = String(data: jsonData, encoding: .utf8)
    print(jsonString ?? "")
} catch {
    print("Error encoding person: \(error)")
}
```

In this example, we define a simple `Person` struct with a `name`, `height`, and `weight`. We intentionally set the `height` to `Double.infinity` and the `weight` to `Double.NaN` to represent non-conforming floating-point values. We then create an instance of `JSONEncoder` and set the `nonConformingFloatEncodingStrategy` property to `.convertToString`. This ensures that the non-conforming values are converted to strings before encoding.

When we encode the `person` instance and print the resulting JSON string, we will see that the non-conforming floating-point values are represented as strings in the JSON output.

By leveraging the `nonConformingFloatEncodingStrategy` property in Swift's `JSONEncoder`, you can handle non-conforming floating-point values gracefully and ensure valid encoding of JSON data.

#Swift #JSONEncoder