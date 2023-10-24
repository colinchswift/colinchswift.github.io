---
layout: post
title: "Nested JSON parsing with multiple levels in Swift"
description: " "
date: 2023-10-24
tags: [json]
comments: true
share: true
---

JSON (JavaScript Object Notation) is a widely used data interchange format, especially in web-based applications. Handling JSON data in Swift is made easier with the Codable protocol that was introduced in Swift 4. Codable allows you to easily encode and decode JSON data to Swift types. However, when dealing with nested JSON structures that have multiple levels, you need to follow a specific approach to parse the data correctly.

In this blog post, we will explore how to parse nested JSON with multiple levels in Swift using the Codable protocol.

## Table of Contents
- [Understanding Nested JSON](#understanding-nested-json)
- [Parsing Nested JSON in Swift](#parsing-nested-json-in-swift)
  - [Defining Codable Model](#defining-codable-model)
  - [Parsing JSON Data](#parsing-json-data)
- [Conclusion](#conclusion)
- [References](#references)

## Understanding Nested JSON

Nested JSON refers to a JSON structure where one or more JSON objects are nested within another JSON object. This nesting creates a hierarchical relationship between the objects.

Consider the following example of a nested JSON structure representing a customer and their orders:

```json
{
  "customer_name": "John Doe",
  "customer_age": 30,
  "orders": [
    {
      "order_id": 1,
      "order_date": "2022-01-01",
      "order_total": 100.0,
      "products": [
        {
          "product_id": 101,
          "product_name": "Product A",
          "product_price": 50.0
        },
        {
          "product_id": 102,
          "product_name": "Product B",
          "product_price": 50.0
        }
      ]
    },
    {
      "order_id": 2,
      "order_date": "2022-02-01",
      "order_total": 75.0,
      "products": [
        {
          "product_id": 201,
          "product_name": "Product C",
          "product_price": 25.0
        },
        {
          "product_id": 202,
          "product_name": "Product D",
          "product_price": 50.0
        }
      ]
    }
  ]
}
```

In this example, the JSON contains a top-level object (`customer_name`, `customer_age`) and an array of nested objects (`orders`). Each `order` object contains its own properties (`order_id`, `order_date`, `order_total`) and another nested array of objects (`products`).

## Parsing Nested JSON in Swift

To parse the nested JSON structure in Swift, we need to define a Codable model that mirrors the JSON structure and implement the necessary decoding logic.

### Defining Codable Model

First, we define the Codable model to mirror the JSON structure. The model should have properties that match the JSON keys and nesting levels.

```swift
struct Customer: Codable {
    let customerName: String
    let customerAge: Int
    let orders: [Order]
}

struct Order: Codable {
    let orderId: Int
    let orderDate: String
    let orderTotal: Double
    let products: [Product]
}

struct Product: Codable {
    let productId: Int
    let productName: String
    let productPrice: Double
}
```

### Parsing JSON Data

To parse the nested JSON data, we use the `JSONDecoder` class provided by Swift's `Foundation` framework. We decode the JSON data into an instance of the top-level Codable model.

```swift
let jsonData = """
... // JSON data
""".data(using: .utf8)

let decoder = JSONDecoder()
do {
    let customer = try decoder.decode(Customer.self, from: jsonData)
    // Access the parsed data
    print(customer.customerName)
    print(customer.customerAge)
    for order in customer.orders {
        print(order.orderId)
        print(order.orderDate)
        print(order.orderTotal)
        for product in order.products {
            print(product.productId)
            print(product.productName)
            print(product.productPrice)
        }
    }
} catch {
    print("Error parsing JSON: \(error.localizedDescription)")
}
```

In this example, we decode the JSON data using the `JSONDecoder` and access the parsed data using dot notation. By iterating over the `customer.orders` array, we can access the nested data.

## Conclusion

Parsing nested JSON with multiple levels in Swift can be achieved easily using the Codable protocol and the JSONDecoder class from the Foundation framework. By defining Codable models that mirror the JSON structure and implementing the necessary decoding logic, we can efficiently parse the data into a Swift object hierarchy.

JSON parsing is a common task in iOS, and understanding how to handle nested JSON structures is essential for building robust and efficient applications.

## References

- [Swift Codable documentation](https://developer.apple.com/documentation/swift/codable)
- [Swift JSONEncoder documentation](https://developer.apple.com/documentation/foundation/jsonencoder)
- [Swift JSONDecoder documentation](https://developer.apple.com/documentation/foundation/jsondecoder)
- [JSON - Wikipedia](https://en.wikipedia.org/wiki/JSON)

#swift #json