---
layout: post
title: "Encoding and decoding custom data types with Codable"
description: " "
date: 2023-09-22
tags: [Swift, Codable]
comments: true
share: true
---

In Swift, one powerful feature is the ability to **encode** and **decode** data types using the `Codable` protocol. The `Codable` protocol provides a convenient way to convert custom data types to and from various representations, such as JSON or Property List.

This blog post will explore how to encode and decode custom data types using `Codable`, and discuss some best practices to ensure smooth and reliable serialization and deserialization.

## Codable Protocol

The `Codable` protocol in Swift is a typealias for two other protocols: `Encodable` and `Decodable`. By conforming to `Codable`, a custom data type automatically adopts both the encoding and decoding functionalities.

To encode a custom data type, it needs to conform to the `Encodable` protocol. Similarly, to decode a custom data type, it needs to conform to the `Decodable` protocol. Implementing these protocols requires implementing their respective methods: `encode(to:)` and `init(from:)`.

## Encoding a Custom Data Type

When encoding a custom data type, implement the `encode(to:)` method from the `Encodable` protocol. This method provides an instance of `Encoder` which encodes the data into the desired format. The most common encoder to use is `JSONEncoder`.

Here's an example of encoding a custom `Person` struct:

```swift
struct Person: Codable {
    var firstName: String
    var lastName: String
    var age: Int
}

let person = Person(firstName: "John", lastName: "Doe", age: 30)
let encoder = JSONEncoder()

do {
    let encodedData = try encoder.encode(person)
    let jsonString = String(data: encodedData, encoding: .utf8)
    print(jsonString)
} catch {
    print("Error encoding person: \(error)")
}
```

In the above code, we define a `Person` struct and make it conform to `Codable` by using the `Codable` protocol. We then create an instance of `Person`, along with an instance of `JSONEncoder` for encoding the data. Finally, we use the `encode` method of the encoder to convert the `person` instance into JSON data.

## Decoding a Custom Data Type

To decode a custom data type, implement the `init(from:)` initializer from the `Decodable` protocol. This initializer provides access to an instance of `Decoder`, which is used to decode the data into the desired format. The most common decoder to use is `JSONDecoder`.

Let's take a look at decoding the JSON data we created above:

```swift
let jsonData = """
{
    "firstName": "John",
    "lastName": "Doe",
    "age": 30
}
""".data(using: .utf8)

let decoder = JSONDecoder()

do {
    let person = try decoder.decode(Person.self, from: jsonData!)
    print(person)
} catch {
    print("Error decoding person: \(error)")
}
```

In this example, we define a `jsonData` constant that contains the JSON data we want to decode. We then create an instance of `JSONDecoder` and use the `decode` method to convert the JSON data into a `Person` instance.

## Best Practices

When working with `Codable`, there are some best practices to keep in mind:

1. **Naming:** Ensure that the property names of your custom data type match the keys in the encoded/decoded data format. You can do this by providing a custom coding key enum for your type.

2. **Handling Optional Values:** If your custom data type has optional properties, you need to handle them properly during encoding and decoding. You can use conditional encoding and decoding to handle optional values.

## Conclusion

Using the `Codable` protocol provides an effortless way to encode and decode custom data types in Swift. By conforming to this protocol and implementing the required methods, you can seamlessly convert between various data representations, such as JSON or Property List. Remember to follow the best practices mentioned above for smooth and reliable encoding and decoding.

#Swift #Codable