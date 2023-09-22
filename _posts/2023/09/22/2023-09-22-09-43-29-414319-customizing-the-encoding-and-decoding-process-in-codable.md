---
layout: post
title: "Customizing the encoding and decoding process in Codable"
description: " "
date: 2023-09-22
tags: [Swift, Codable]
comments: true
share: true
---

With the introduction of Codable in Swift 4, working with JSON data has become much easier. Codable provides a simple and convenient way to convert Swift objects to and from JSON. In most cases, the default coding keys and strategies work well. However, there may be situations where you need to customize the encoding and decoding process. In this blog post, we will explore how to do just that.

## Customizing Coding Keys

By default, Codable maps properties in your Swift structs and classes to JSON keys with the same name. However, if the JSON keys have a different naming convention, you can customize the coding keys.

To customize the coding keys, you need to implement the `CodingKey` protocol. The protocol requires you to provide a `stringValue` property that contains the actual JSON key string. 

Here's an example:

```swift
struct Person: Codable {
    var firstName: String
    var lastName: String
    
    private enum CodingKeys: String, CodingKey {
        case firstName = "first_name"
        case lastName = "last_name"
    }
}
```

In this example, we have a `Person` struct with `firstName` and `lastName` properties. The `CodingKeys` enum is defined as a private nested enum within the struct. Each case in the enum represents a property we want to encode and decode, and the raw value of the case is set to the corresponding JSON key.

Now, when you encode or decode the `Person` struct, the custom coding keys will be used.

## Customizing Encoding and Decoding Strategies

In addition to custom coding keys, you can also customize the encoding and decoding strategies. The default strategy is to use the property names as-is when converting between Swift objects and JSON. However, you may want to apply some transformations to the property names.

To customize the encoding and decoding strategies, you can use the `keyEncodingStrategy` and `keyDecodingStrategy` properties of the `JSONEncoder` and `JSONDecoder` classes. These properties allow you to specify a closure that transforms the property names.

Here's an example:

```swift
struct Person: Codable {
    var firstName: String
    var lastName: String
    
    private enum CodingKeys: String, CodingKey {
        case firstName
        case lastName
    }
    
    static let JSONEncoder: JSONEncoder = {
        let encoder = JSONEncoder()
        encoder.keyEncodingStrategy = .custom { keys in
            var container = keys.last!
            let key = keys.dropLast().joined(separator: "_")
            container.stringValue = key.uppercased()
            return container
        }
        return encoder
    }()
    
    static let JSONDecoder: JSONDecoder = {
        let decoder = JSONDecoder()
        decoder.keyDecodingStrategy = .custom { keys in
            let key = keys.reduce("", { $0 + $1.stringValue })
            return CodingKeys(stringValue: key.lowercased())!
        }
        return decoder
    }()
}
```

In this example, we have a `Person` struct with `firstName` and `lastName` properties. We define the encoding and decoding strategies as static properties on the struct. In each strategy, we use a closure to transform the property names. In the encoding strategy, we join the keys with underscores and convert the result to uppercase. In the decoding strategy, we convert the key to lowercase and use it to find the corresponding `CodingKeys` enum case.

By using custom coding keys and encoding/decoding strategies, you can have finer-grained control over the JSON encoding and decoding process in your Codable structures.

## Conclusion

Codable in Swift provides a powerful way to work with JSON data. By customizing the coding keys and encoding/decoding strategies, you can make your Codable structures work with JSON that follows different naming conventions or requires additional transformations. This flexibility allows you to handle a wide range of JSON formats effectively.

#Swift #Codable