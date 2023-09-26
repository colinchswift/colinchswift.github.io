---
layout: post
title: "Encoding and decoding Swift objects with default values using Codable"
description: " "
date: 2023-09-22
tags: [CodingTips]
comments: true
share: true
---

One of the powerful features in Swift is the Codable protocol, which provides a convenient way to encode and decode Swift objects to and from various data formats like JSON and Property Lists. However, when encoding or decoding objects that have optional properties with default values, some additional steps are required to handle the default values correctly.

## The Issue with Default Values

By default, when you declare a property in Swift, it can have an optional type and a default value assigned to it. For example:

```swift
struct Person: Codable {
    var name: String = "John Doe"
    var age: Int?
}
```

In this case, the `name` property has a default value of "John Doe" and the `age` property is optional. 

When encoding this object, you might expect that the default value of "John Doe" for the `name` property would be included in the encoded data, but that's not the case. By default, the Codable protocol skips properties that have default values when encoding an object.

## Encoding with Default Values

To ensure that properties with default values are included in the encoded data, you can implement the `encode(to:)` method from the `Encodable` protocol to manually encode the properties, even if they have default values. Here's an example:

```swift
struct Person: Codable {
    var name: String = "John Doe"
    var age: Int?
    
    enum CodingKeys: String, CodingKey {
        case name
        case age
    }
    
    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(name, forKey: .name)
        try container.encode(age, forKey: .age)
    }
}
```

In this example, we manually encode both the `name` and `age` properties, regardless of their default values. Now, when you encode an instance of the `Person` struct, the default value for the `name` property will be included in the encoded data.

## Decoding with Default Values

When decoding objects that have properties with default values, Codable automatically assigns the default value to the property if it is missing in the decoded data. However, if the property is explicitly set to `nil` in the decoded data, the default value won't be used.

To handle this scenario and ensure that the default values are assigned correctly, you can implement the `init(from:)` method from the `Decodable` protocol. Here's an example:

```swift
struct Person: Codable {
    var name: String = "John Doe"
    var age: Int?
    
    enum CodingKeys: String, CodingKey {
        case name
        case age
    }
    
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        name = try container.decode(String.self, forKey: .name)
        age = try container.decodeIfPresent(Int.self, forKey: .age)
        
        // If `age` is nil and not present in the decoded data,
        // assign the default value manually
        if age == nil {
            age = 18
        }
    }
}
```

In this example, we first decode the `name` property from the decoded container. Then, we use `decodeIfPresent(_:forKey:)` to decode the `age` property, which assigns `nil` if the key is missing in the decoded data. After that, we check if `age` is `nil` and manually assign the default value of 18 if it is.

By implementing the `init(from:)` method like this, you can ensure that properties with default values are assigned correctly during object decoding.

## Conclusion

When encoding and decoding Swift objects with default values using the Codable protocol, additional steps are required to handle the default values correctly. By implementing the `encode(to:)` method for encoding and the `init(from:)` method for decoding, you can ensure that properties with default values are included in the encoded data and assigned correctly during object decoding.

##### #Swift #CodingTips