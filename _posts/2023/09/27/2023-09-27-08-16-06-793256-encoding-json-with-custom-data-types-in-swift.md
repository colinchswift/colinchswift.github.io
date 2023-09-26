---
layout: post
title: "Encoding JSON with custom data types in Swift"
description: " "
date: 2023-09-27
tags: [jsonencoding]
comments: true
share: true
---

In Swift, you can use the `Codable` protocol to easily encode and decode JSON data. However, when it comes to encoding custom data types that are not natively supported by `Codable`, you need to implement some additional code. In this blog post, we will explore how to encode custom data types to JSON in Swift.

## Create a Custom Type

Let's say we have a custom data type called `Person`, which has properties like `name`, `age`, and `address`. We want to encode an instance of this `Person` type to JSON.

```swift
struct Person {
    var name: String
    var age: Int
    var address: String
}
```

## Conform to the Encodable Protocol

To encode a custom data type, it needs to conform to the `Encodable` protocol. The `Encodable` protocol requires you to implement the `encode(to: Encoder)` method.

```swift
struct Person: Encodable {
    var name: String
    var age: Int
    var address: String
    
    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(name, forKey: .name)
        try container.encode(age, forKey: .age)
        try container.encode(address, forKey: .address)
    }
    
    enum CodingKeys: String, CodingKey {
        case name
        case age
        case address
    }
}
```

In the `encode(to: Encoder)` method, we create a container using the `encoder` parameter. We then encode each property of the `Person` struct by calling the `encode(_:forKey:)` method on the container.

## Encode the Custom Type to JSON

Now that we have conformed our `Person` struct to the `Encodable` protocol, we can easily encode an instance of `Person` to JSON.

```swift
let person = Person(name: "John Doe", age: 30, address: "123 Main St")
let jsonEncoder = JSONEncoder()
let jsonData = try jsonEncoder.encode(person)
let jsonString = String(data: jsonData, encoding: .utf8)
print(jsonString ?? "")
```

In this example, we create an instance of `Person` called `person`. We then create an instance of `JSONEncoder` to encode the `person` object. After encoding, we convert the resulting `Data` to a string and print it.

When you run this code, the output will be a JSON representation of the `person` object:

```json
{
    "name": "John Doe",
    "age": 30,
    "address": "123 Main St"
}
```

## Conclusion

Encoding custom data types to JSON in Swift is easy, thanks to the `Encodable` protocol. By conforming your custom data type to `Encodable` and implementing the `encode(to: Encoder)` method, you can easily encode your objects to JSON.

#swift #jsonencoding