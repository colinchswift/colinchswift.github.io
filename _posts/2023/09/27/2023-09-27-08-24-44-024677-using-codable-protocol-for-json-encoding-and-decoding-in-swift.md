---
layout: post
title: "Using Codable protocol for JSON encoding and decoding in Swift"
description: " "
date: 2023-09-27
tags: [json, encoding]
comments: true
share: true
---

When working with JSON data in Swift, one of the most efficient ways to handle encoding and decoding is by using the `Codable` protocol. Introduced in Swift 4, this protocol allows you to automatically convert your custom types to and from JSON without writing additional serialization or deserialization code. In this blog post, we will explore how to use the `Codable` protocol for JSON encoding and decoding in Swift.

## Creating a Codable Type

To use the `Codable` protocol, you need to declare your custom type as being `Codable` or conforming to `Codable`. This can be achieved by implementing the `Codable` protocol, which is actually a combination of two separate protocols: `Encodable` and `Decodable`.

Let's consider an example where we have a `Person` struct with `name` and `age` properties that we want to encode and decode in JSON.

```swift
struct Person: Codable {
    let name: String
    let age: Int
}
```

By declaring `Person` as `Codable`, we automatically get the implementations for `Encodable` and `Decodable`, saving us from writing the encoding and decoding logic ourselves.

## Encoding to JSON

To encode an instance of our `Person` struct to JSON, we can use the `JSONEncoder` class provided by the Swift Foundation framework. The `JSONEncoder` converts our structured data into a JSON object.

```swift
let person = Person(name: "John Doe", age: 30)

let encoder = JSONEncoder()
encoder.outputFormatting = .prettyPrinted

do {
    let jsonData = try encoder.encode(person)
    if let jsonString = String(data: jsonData, encoding: .utf8) {
        print(jsonString)
    }
} catch {
    print("Error encoding: \(error.localizedDescription)")
}
```

In the above code snippet, we first create an instance of `Person` with some sample data. Then, we initialize an instance of `JSONEncoder` and set the `outputFormatting` property to `.prettyPrinted` to get a nicely formatted JSON string. Finally, we encode the `person` object and print the resulting JSON string.

## Decoding JSON

To decode a JSON string back into our `Person` struct, we use the `JSONDecoder` class provided by the Swift Foundation framework.

```swift
let jsonString = """
    {
        "name": "John Doe",
        "age": 30
    }
    """

let decoder = JSONDecoder()

do {
    let jsonData = jsonString.data(using: .utf8)!
    let person = try decoder.decode(Person.self, from: jsonData)
    print(person)
} catch {
    print("Error decoding: \(error.localizedDescription)")
}
```

In the above example, we first define a JSON string representing our `Person` data. Then, we initialize an instance of `JSONDecoder` and decode the JSON data using the `decode(_:from:)` method. We specify the target type as `Person.self`, and the `fromJson` parameter as the JSON data. Finally, we print the decoded `person` object.

## Conclusion

The `Codable` protocol in Swift provides an elegant way to handle JSON encoding and decoding without writing cumbersome serialization or deserialization code. By conforming to the `Codable` protocol, you can easily encode and decode your custom types to and from JSON using the `JSONEncoder` and `JSONDecoder` classes. This not only simplifies the process, but also makes your code more maintainable and robust.

#swift #json #encoding #decoding