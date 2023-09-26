---
layout: post
title: "Working with Keyed and Unkeyed containers in Codable"
description: " "
date: 2023-09-22
tags: [Coding]
comments: true
share: true
---

The Codable protocol in Swift allows us to easily encode and decode data to and from different representations such as JSON or property lists. When working with Codable, we often encounter two different types of containers: keyed and unkeyed.

## Keyed Containers

Keyed containers are used when we have a dictionary-like representation where the data is stored with keys associated with each value. In Swift, this is usually represented as a JSON object or a property list dictionary.

To work with keyed containers, we need to define our data model as a struct or class that conforms to the Codable protocol. Inside the `init(from decoder: Decoder)` method, we can access the keyed container using the `decoder.container(keyedBy: CodingKeys.self)` method.

For example, let's say we have a JSON representation of a `Person` object:

```swift
struct Person: Codable {
    let name: String
    let age: Int
    
    enum CodingKeys: String, CodingKey {
        case name
        case age
    }
    
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        name = try container.decode(String.self, forKey: .name)
        age = try container.decode(Int.self, forKey: .age)
    }
}
```

In the above code, we define our `Person` struct with two properties: `name` and `age`. We also define an enum `CodingKeys` that conforms to the `String` type and specifies the coding keys corresponding to each property. Inside the `init(from decoder: Decoder)` method, we access the keyed container using `container(keyedBy:)` and decode the values using `decode(_:forKey:)` method.

## Unkeyed Containers

Unkeyed containers are used when we have an array-like representation where the data is stored without keys, in a specific order. In Swift, this is usually represented as a JSON array or a property list array.

To work with unkeyed containers, we need to define our data model as a struct or class that conforms to the Codable protocol. Inside the `init(from decoder: Decoder)` method, we can access the unkeyed container using the `decoder.unkeyedContainer()` method.

For example, let's say we have a JSON representation of a `Playlist` object:

```swift
struct Playlist: Codable {
    let name: String
    let songs: [String]
    
    init(from decoder: Decoder) throws {
        var container = try decoder.unkeyedContainer()
        name = try container.decode(String.self)
        songs = try container.decode([String].self)
    }
}
```

In the above code, we define our `Playlist` struct with two properties: `name` and `songs`. Inside the `init(from decoder: Decoder)` method, we access the unkeyed container using `unkeyedContainer()` and decode the values using `decode(_:)` method.

## Conclusion

Working with keyed and unkeyed containers in Codable allows us to easily encode and decode data in different formats. By following the syntax and techniques discussed above, you can effectively work with both types of containers and handle complex data structures with ease.

#Swift #Coding