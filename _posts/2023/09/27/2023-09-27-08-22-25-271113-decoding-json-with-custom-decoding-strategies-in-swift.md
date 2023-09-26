---
layout: post
title: "Decoding JSON with custom decoding strategies in Swift"
description: " "
date: 2023-09-27
tags: [json]
comments: true
share: true
---

JSON (JavaScript Object Notation) is a widely used format for exchanging data between a server and a client. In Swift, the Codable protocol provides an easy way to encode and decode JSON data. By default, Codable uses a property-based decoding strategy, where the JSON keys must match the property names in your Swift structs or classes.

However, there may be cases where the JSON keys do not match the property names exactly or follow a different naming convention. In such situations, you can use custom decoding strategies to decode JSON data. Let's explore how to implement custom decoding strategies in Swift.

## 1. Renaming Keys

One common scenario is when the JSON keys have different names than the corresponding property names in Swift. You can use the `CodingKeys` enumeration inside your Codable struct or class to provide custom key mappings.

```swift
struct Person: Codable {
    let name: String
    let age: Int

    private enum CodingKeys: String, CodingKey {
        case name = "person_name"
        case age = "person_age"
    }
}
```

In the above code, we have defined a struct `Person` with properties `name` and `age`. By implementing the `CodingKeys` enumeration and providing custom raw values, we can map the JSON keys `person_name` and `person_age` to `name` and `age`, respectively.

## 2. Custom Decoding Logic

In some cases, you may need more complex decoding logic to handle JSON data. You can achieve this by implementing the `init(from:)` initializer of `Decodable` protocol. This allows you to manually decode individual properties and perform any necessary transformations.

```swift
struct Article: Codable {
    let title: String
    let publicationDate: Date

    private enum CodingKeys: String, CodingKey {
        case title
        case publicationDateText = "pub_date"
    }

    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        title = try container.decode(String.self, forKey: .title)

        let publicationDateText = try container.decode(String.self, forKey: .publicationDateText)
        let dateFormatter = DateFormatter()
        dateFormatter.dateFormat = "yyyy-MM-dd"
        publicationDate = dateFormatter.date(from: publicationDateText) ?? Date()
    }
}
```

In the above code, we have a `PublicationDate` property in our JSON data represented as a string in the format "yyyy-MM-dd". By implementing the `init(from:)` initializer, we can decode the `publicationDateText` from the JSON and convert it to a `Date` object using a `DateFormatter`.

These are just a few examples of the possibilities when working with custom decoding strategies in Swift. The Codable protocol provides a powerful mechanism for decoding JSON data, allowing you to adapt to different JSON representations.

#swift #json