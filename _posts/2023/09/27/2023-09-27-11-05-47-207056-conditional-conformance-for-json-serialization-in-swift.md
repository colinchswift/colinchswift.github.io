---
layout: post
title: "Conditional conformance for JSON serialization in Swift"
description: " "
date: 2023-09-27
tags: [JSON]
comments: true
share: true
---

As a Swift developer, you may often find yourself needing to convert Swift objects to JSON and vice versa. While Swift's `Codable` protocol provides a convenient way to handle JSON serialization, there are cases where you need more fine-grained control over the process.

One common requirement is to conditionally include or exclude certain properties from the JSON representation based on some condition. For instance, you may only want to include properties in the JSON payload if they have a non-nil value. This is where conditional conformance comes in handy.

## Conditional Conformance with Optional Properties

Let's say we have a `Person` struct that represents a person with a name, age, and address:

```swift
struct Person {
    let name: String
    let age: Int
    let address: String?
}
```

If we simply conform to the `Codable` protocol, both the `name` and `age` properties will always be included in the resulting JSON, even if `address` is `nil`. To conditionally include the `address` property only if it has a value, we can define a custom container:

```swift
struct Person: Codable {
    let name: String
    let age: Int
    let address: String?
    
    private enum CodingKeys: String, CodingKey {
        case name, age
        case address = "address"
    }

    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(name, forKey: .name)
        try container.encode(age, forKey: .age)

        if let address = address {
            try container.encode(address, forKey: .address)
        }
    }
}
```

By providing a custom `encode(to:)` method, we can selectively encode the `address` property only if it has a non-nil value.

## Conditional Conformance with Dynamic Properties

In some cases, you may have a dynamic number of properties that need to be conditionally included in the resulting JSON. For example, consider a `Person` struct with additional `hobbies`:

```swift
struct Person {
    let name: String
    let age: Int
    let hobbies: [String]
}
```

If you want to exclude `hobbies` from the JSON when the array is empty, you can achieve this through conditional conformance by extending the `KeyedEncodingContainer`:

```swift
extension KeyedEncodingContainer {

    mutating func encodeIfPresent(_ value: [String], forKey key: KeyedEncodingContainer.Key) throws {
        if !value.isEmpty {
            try encode(value, forKey: key)
        }
    }
}

extension Person: Codable {

    private enum CodingKeys: String, CodingKey {
        case name, age, hobbies
    }

    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(name, forKey: .name)
        try container.encode(age, forKey: .age)
        try container.encodeIfPresent(hobbies, forKey: .hobbies)
    }
}
```

Here, we extended the `KeyedEncodingContainer` to provide an `encodeIfPresent(_:forKey:)` method that checks if the `hobbies` array is empty before encoding it.

## Conclusion

By leveraging conditional conformance in Swift, you can have more control over JSON serialization. Whether it's excluding nil values or dynamically omitting properties based on conditions, conditional conformance provides a flexible solution to handle different serialization requirements.

#Swift #JSON #Serialization