---
layout: post
title: "Using JSONDecoder's keyDecodingStrategy in Swift"
description: " "
date: 2023-09-27
tags: []
comments: true
share: true
---

When working with JSON data in Swift, the built-in `JSONDecoder` class is a powerful tool for parsing and decoding JSON into Swift data structures. One feature that comes in handy is the `keyDecodingStrategy` property, which allows you to customize how the decoder matches JSON keys to Swift property names.

## Why use `keyDecodingStrategy`?

By default, `JSONDecoder` expects the JSON keys to match the Swift property names exactly. However, this can be problematic if the JSON keys use a different naming convention, such as snake_case or camelCase. In such cases, decoding the JSON data without modification would result in properties with incorrect values or even decoding failures.

## Customizing `keyDecodingStrategy`

To overcome this issue, Swift provides different strategies for mapping JSON keys to Swift property names. Here are a few commonly used strategies:

1. **`convertFromSnakeCase`**: This strategy converts the snake_case JSON keys to camelCase Swift property names. For example, `first_name` in JSON would be mapped to `firstName` in Swift.

```swift
let json = """
{
    "first_name": "John",
    "last_name": "Doe"
}
""".data(using: .utf8)

let decoder = JSONDecoder()
decoder.keyDecodingStrategy = .convertFromSnakeCase

do {
    let person = try decoder.decode(Person.self, from: json)
    print(person.firstName) // Output: John
    print(person.lastName) // Output: Doe
} catch {
    print("Error decoding JSON: \(error)")
}
```

2. **`useDefaultKeys`**: This strategy matches the JSON keys exactly with the Swift property names. This is the default behavior of `JSONDecoder`, but you can explicitly set it if needed.

```swift
let json = """
{
    "firstName": "John",
    "lastName": "Doe"
}
""".data(using: .utf8)

let decoder = JSONDecoder()
decoder.keyDecodingStrategy = .useDefaultKeys

do {
    let person = try decoder.decode(Person.self, from: json)
    print(person.firstName) // Output: John
    print(person.lastName) // Output: Doe
} catch {
    print("Error decoding JSON: \(error)")
}
```

3. **Custom Strategies**: You can also create your own custom strategy by implementing the `CodingKey` protocol. This allows you to specify how JSON keys should be mapped to Swift property names.

```swift
struct CustomKeysStrategy: CodingKey {
    var stringValue: String
    init?(stringValue: String) {
        self.stringValue = stringValue
    }
    var intValue: Int? { return nil }
    init?(intValue: Int) { return nil }
}

let json = """
{
    "f_name": "John",
    "l_name": "Doe"
}
""".data(using: .utf8)

let decoder = JSONDecoder()
decoder.keyDecodingStrategy = .custom { keys -> CodingKey in
    let key = keys.last!.stringValue.replacingOccurrences(of: "_", with: "")
    return CustomKeysStrategy(stringValue: key)!
}

do {
    let person = try decoder.decode(Person.self, from: json)
    print(person.firstName) // Output: John
    print(person.lastName) // Output: Doe
} catch {
    print("Error decoding JSON: \(error)")
}
```

With the flexibility provided by `keyDecodingStrategy`, you can effortlessly handle JSON data with different naming conventions and map them to your Swift structs or classes. Choose the appropriate strategy that fits your JSON key naming convention and benefit from clean and readable code.