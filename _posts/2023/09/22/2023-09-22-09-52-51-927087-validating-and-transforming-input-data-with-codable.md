---
layout: post
title: "Validating and transforming input data with Codable"
description: " "
date: 2023-09-22
tags: [SwiftCoding, DataValidation]
comments: true
share: true
---

When working with data in your applications, it's important to ensure that the input data is valid and can be transformed into the expected format. In Swift, the `Codable` protocol provides a convenient way to validate and transform input data into your custom Swift types.

## What is Codable?

`Codable` is a protocol introduced in Swift 4 that combines the functionality of `Encodable` and `Decodable`. It allows you to encode (transform into a serialized format) and decode (transform back into the original format) your Swift types with minimal effort.

## Validating input data

Before decoding input data into a Swift type, you may want to validate its contents to ensure it meets certain criteria. To do this, you can implement the `init(from decoder: Decoder) throws` initializer from the `Decodable` protocol. Within this initializer, you can perform any necessary validation checks and throw an error if the input data is not valid.

For example, suppose you have a `User` struct with a `username` property that must be at least 5 characters long. You can add the validation logic like this:

```swift
struct User: Codable {
    let username: String
    
    private enum CodingKeys: String, CodingKey {
        case username
    }
    
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        
        // Perform validation checks
        guard let username = try container.decodeIfPresent(String.self, forKey: .username),
              username.count >= 5 else {
            throw DecodingError.dataCorruptedError(forKey: .username,
                                                   in: container,
                                                   debugDescription: "Username must be at least 5 characters long")
        }
        
        self.username = username
    }
}
```

In the above code, we use the `decodeIfPresent(_:forKey:)` method to safely retrieve the value for the `"username"` key from the data container. If the username exists and its length is less than 5 characters, we throw a `DecodingError` with a descriptive error message.

## Transforming input data

In addition to validation, you may need to transform the input data to match your desired data structure. This can be achieved by implementing the `init(from decoder: Decoder) throws` initializer. Within this initializer, you have the flexibility to map and transform key-value pairs from the input data using any custom logic.

For example, suppose you receive a JSON payload with the `"age"` field represented as a string. You can transform it into an integer within the initializer like this:

```swift
struct User: Codable {
    let age: Int
    
    private enum CodingKeys: String, CodingKey {
        case age
    }
    
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        
        // Transform and validate the age field
        let ageString = try container.decode(String.self, forKey: .age)
        guard let age = Int(ageString) else {
            throw DecodingError.dataCorruptedError(forKey: .age,
                                                   in: container,
                                                   debugDescription: "Invalid age format")
        }
        
        self.age = age
    }
}
```

In this code, we decode the `"age"` field as a string and attempt to convert it into an integer using `Int(ageString)`. If the conversion fails, we throw a `DecodingError` with a descriptive error message.

## Conclusion

Validating and transforming input data is an important step in working with external data sources or user input. By implementing the `Codable` protocol in your Swift types, you can easily validate and transform input data, ensuring the integrity and correctness of your application's data. 

#SwiftCoding #DataValidation