---
layout: post
title: "Parsing JSON with numeric values in Swift"
description: " "
date: 2023-10-24
tags: []
comments: true
share: true
---

When parsing JSON data in Swift, it is common to encounter numeric values. JSON allows multiple types for representing numbers, such as integers, decimals, and scientific notation. In this blog post, we will explore how to parse different numeric values from JSON in Swift.

## 1. Parsing JSON with integer values

If the JSON data contains integer values, we can use the `Int` type in Swift to parse and store them. For example, suppose we have the following JSON data:

```json
{
  "age": 25,
  "score": 98
}
```

We can parse this JSON data in Swift as follows:

```swift
struct User: Codable {
    let age: Int
    let score: Int
}

let json = """
{
  "age": 25,
  "score": 98
}
"""

let jsonData = json.data(using: .utf8)!
let decoder = JSONDecoder()
let user = try! decoder.decode(User.self, from: jsonData)

print("Age: \(user.age)")
print("Score: \(user.score)")
```

The output will be:

```
Age: 25
Score: 98
```

## 2. Parsing JSON with decimal values

When dealing with JSON that contains decimal values, we can use the `Double` or `Float` type in Swift to parse and store them. For example, consider the following JSON data:

```json
{
  "price": 19.99,
  "rating": 4.5
}
```

We can parse this JSON data in Swift like this:

```swift
struct Product: Codable {
    let price: Double
    let rating: Double
}

let json = """
{
  "price": 19.99,
  "rating": 4.5
}
"""

let jsonData = json.data(using: .utf8)!
let decoder = JSONDecoder()
let product = try! decoder.decode(Product.self, from: jsonData)

print("Price: \(product.price)")
print("Rating: \(product.rating)")
```

The output will be:

```
Price: 19.99
Rating: 4.5
```

## 3. Parsing JSON with scientific notation values

Sometimes, JSON data may contain numbers written in scientific notation. In Swift, we can use the `Double` type to parse and store these values. For example, consider the following JSON data:

```json
{
  "distance": 1.23e6,
  "speed": 3.4e-3
}
```

We can parse this JSON data in Swift as shown below:

```swift
struct Measurement: Codable {
    let distance: Double
    let speed: Double
}

let json = """
{
  "distance": 1.23e6,
  "speed": 3.4e-3
}
"""

let jsonData = json.data(using: .utf8)!
let decoder = JSONDecoder()
let measurement = try! decoder.decode(Measurement.self, from: jsonData)

print("Distance: \(measurement.distance)")
print("Speed: \(measurement.speed)")
```

The output will be:

```
Distance: 1230000.0
Speed: 0.0034
```

## Conclusion

By understanding the different ways numeric values can be represented in JSON, we can easily parse them in Swift. Whether it's integers, decimals, or scientific notation, Swift provides appropriate data types like `Int`, `Double`, and `Float` to store these values accurately.