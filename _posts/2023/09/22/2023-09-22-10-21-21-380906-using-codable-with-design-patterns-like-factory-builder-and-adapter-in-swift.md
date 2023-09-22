---
layout: post
title: "Using Codable with design patterns like factory, builder, and adapter in Swift"
description: " "
date: 2023-09-22
tags: [Swift, Codable]
comments: true
share: true
---

Design patterns are an essential part of software development, providing proven solutions to common problems. In Swift, the Codable protocol is a powerful tool for efficient serialization and deserialization of objects. It allows us to easily convert Swift objects to different formats like JSON and plist, and vice versa.

In this blog post, we will explore how to use Codable in combination with popular design patterns like Factory, Builder, and Adapter. These patterns can help us organize our code and make it more maintainable and flexible.

## Factory Pattern

The Factory pattern is useful when we want to create objects of different types based on a common interface or base class. With Codable, we can take advantage of the `CodableFactory` pattern to dynamically create objects from a JSON representation.

```swift
protocol CodableFactory {
    static func create<T: Codable>(from json: [String: Any]) -> T?
}

struct User: Codable {
    let name: String
    let age: Int
    
    enum CodingKeys: String, CodingKey {
        case name
        case age
    }
}

extension User: CodableFactory {
    static func create<T>(from json: [String : Any]) -> T? where T : Decodable {
        guard let data = try? JSONSerialization.data(withJSONObject: json, options: []),
              let user = try? JSONDecoder().decode(User.self, from: data) as? T else {
            return nil
        }
        return user
    }
}

// Usage
let jsonData = """
{
    "name": "John Doe",
    "age": 25
}
""".data(using: .utf8)!

let json = try! JSONSerialization.jsonObject(with: jsonData, options: []) as! [String: Any]

if let user: User = User.create(from: json) {
    print(user.name) // John Doe
    print(user.age) // 25
}
```
The `CodableFactory` protocol defines a method that takes a JSON dictionary and returns an instance of a specific type. In the `User` type, we implement this protocol and provide the JSON decoding logic within the `create` method.

## Builder Pattern

The Builder pattern is useful when we want to construct complex objects with a step-by-step process. With Codable, we can leverage the `CodableBuilder` pattern to construct objects while decoding from JSON.

```swift
struct Book: Codable {
    let title: String
    let author: String
    let pageCount: Int
    
    enum CodingKeys: String, CodingKey {
        case title
        case author
        case pageCount
    }
    
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        
        // Handle optional decoding
        title = try container.decodeIfPresent(String.self, forKey: .title) ?? ""
        author = try container.decodeIfPresent(String.self, forKey: .author) ?? ""
        pageCount = try container.decodeIfPresent(Int.self, forKey: .pageCount) ?? 0
    }
}

// Usage
let jsonData = """
{
    "title": "The Swift Programming Language",
    "author": "Apple Inc.",
    "pageCount": 400
}
""".data(using: .utf8)!

let decoder = JSONDecoder()
let book = try! decoder.decode(Book.self, from: jsonData)

print(book.title) // The Swift Programming Language
print(book.author) // Apple Inc.
print(book.pageCount) // 400
```

In the `Book` type, we provide a custom `init` method that allows us to handle optional decoding of properties. This gives us flexibility during decoding, allowing us to assign default values when certain properties are not present in the JSON.

## Adapter Pattern

The Adapter pattern is useful when we want to map data from one format to another. With Codable, we can use the `CodableAdapter` pattern to transform JSON data before decoding it into Swift objects.

```swift
struct Hotel: Codable {
    let name: String
    let location: String
    let starRating: Double
    
    enum CodingKeys: String, CodingKey {
        case name
        case location
        case starRating = "rating"
    }
}

// Usage
let jsonData = """
{
    "name": "Grand Hotel",
    "location": "New York",
    "rating": 4.5
}
""".data(using: .utf8)!

let decoder = JSONDecoder()
let hotel = try! decoder.decode(Hotel.self, from: jsonData)

print(hotel.name) // Grand Hotel
print(hotel.location) // New York
print(hotel.starRating) // 4.5
```

In the `Hotel` type, we use the `CodingKeys` enum to specify the mapping between the JSON keys and the Swift struct properties. By using `starRating = "rating"`, we can map the `rating` key in the JSON to the `starRating` property in Swift.

## Conclusion

In this blog post, we explored how to use the Codable protocol in combination with popular design patterns like Factory, Builder, and Adapter. By leveraging these design patterns, we can make our code more maintainable, flexible, and easy to read. Codable provides a great way to perform efficient serialization and deserialization, making it easier than ever to work with different data formats in Swift.

#Swift #Codable #FactoryPattern #BuilderPattern #AdapterPattern