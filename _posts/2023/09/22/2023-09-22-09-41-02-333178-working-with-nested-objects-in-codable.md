---
layout: post
title: "Working with nested objects in Codable"
description: " "
date: 2023-09-22
tags: []
comments: true
share: true
---

When working with JSON data in Swift, Codable is a powerful tool for parsing and encoding data. It allows you to easily create model objects that can be converted to and from JSON. However, dealing with nested objects can be a bit more challenging. In this blog post, we will explore how to work with nested objects in Codable.

## Defining Nested Objects ##

To work with nested objects in Codable, you need to define the structure of your data model. Let's say we have a JSON object that looks like this:

```swift
{
  "name": "John Doe",
  "age": 30,
  "address": {
    "street": "123 Main St",
    "city": "New York",
    "state": "NY"
  }
}
```

To represent this data in Swift, we can define a `Person` struct that has an `Address` struct nested inside it. Here's an example:

```swift
struct Person: Codable {
  let name: String
  let age: Int
  let address: Address
}

struct Address: Codable {
  let street: String
  let city: String
  let state: String
}
```

## Conforming to Codable ##

To make our `Person` and `Address` structs Codable, we need to conform to the `Codable` protocol. In most cases, simply adding `Codable` to the struct declaration is enough, as long as all the properties of the struct are themselves Codable:

```swift
struct Person: Codable {
  // properties
  
  enum CodingKeys: String, CodingKey {
    // coding keys
  }
  
  // implementation
}

struct Address: Codable {
  // properties
  
  enum CodingKeys: String, CodingKey {
    // coding keys
  }
  
  // implementation
}
```

If you need to map the JSON keys to different property names in your structs, you can define a `CodingKeys` enum and provide custom keys for each property. This is especially useful when the property names in the JSON do not match your Swift struct's property names.

## Encoding and Decoding ##

With our `Person` and `Address` structs conforming to Codable, we can now easily encode and decode JSON data. Here's an example of encoding a `Person` object to JSON:

```swift
let person = Person(name: "John Doe", age: 30, address: Address(street: "123 Main St", city: "New York", state: "NY"))

let encoder = JSONEncoder()
encoder.outputFormatting = .prettyPrinted

do {
  let jsonData = try encoder.encode(person)
  if let jsonString = String(data: jsonData, encoding: .utf8) {
    print(jsonString)
  }
} catch {
  print(error.localizedDescription)
}
```

And here's an example of decoding JSON data into a `Person` object:

```swift
let jsonString = """
{
  "name": "John Doe",
  "age": 30,
  "address": {
    "street": "123 Main St",
    "city": "New York",
    "state": "NY"
  }
}
"""

let decoder = JSONDecoder()

do {
  let jsonData = jsonString.data(using: .utf8)!
  let person = try decoder.decode(Person.self, from: jsonData)
  print(person)
} catch {
  print(error.localizedDescription)
}
```

## Conclusion ##

Working with nested objects in Codable is fairly straightforward once you understand the basics. By defining nested structs that conform to Codable and leveraging the power of the `Codable` protocol, you can easily parse and encode JSON data in Swift.