---
layout: post
title: "Parsing JSON with different data types in Swift"
description: " "
date: 2023-10-24
tags: [JSON]
comments: true
share: true
---

In Swift, parsing JSON with different data types can be a common requirement when working with APIs or data from external sources. JSON (JavaScript Object Notation) is a lightweight data-interchange format and is widely used for transferring data between client and server.

When parsing JSON in Swift, it's essential to correctly handle different data types, such as strings, numbers, booleans, arrays, and dictionaries, to ensure accurate representation of the data.

## 1. Decoding JSON using Codable protocol

One approach to parse JSON with different data types in Swift is by leveraging the `Codable` protocol introduced in Swift 4. The `Codable` protocol provides a convenient way to encode and decode custom types to and from JSON.

Consider the following JSON data:

```swift
{
    "name": "John Doe",
    "age": 28,
    "isEmployed": true,
    "favoriteNumbers": [1, 2, 3],
    "address": {
        "street": "123 Main St",
        "city": "New York"
    }
}
```

To parse this JSON, create a corresponding Swift struct or class conforming to the `Codable` protocol:

```swift
struct Person: Codable {
    let name: String
    let age: Int
    let isEmployed: Bool
    let favoriteNumbers: [Int]
    let address: Address
}

struct Address: Codable {
    let street: String
    let city: String
}
```

Now, use the `JSONDecoder` to decode the JSON into your custom struct:

```swift
let jsonString = """
    {"name": "John Doe", "age": 28, "isEmployed": true, "favoriteNumbers": [1, 2, 3], "address": {"street": "123 Main St", "city": "New York"}}
"""

let jsonData = jsonString.data(using: .utf8)!
let decoder = JSONDecoder()

do {
    let person = try decoder.decode(Person.self, from: jsonData)
    print(person)
} catch {
    print("Error decoding JSON: \(error.localizedDescription)")
}
```

With the `Codable` protocol, the decoding process handles various data types automatically as long as they match the structure defined in your Swift struct or class.

## 2. Manual JSON parsing with JSONSerialization

Another approach to parsing JSON with different data types in Swift is by manually using `JSONSerialization`, which is available in Foundation framework.

```swift
let jsonString = """
    {"name": "John Doe", "age": 28, "isEmployed": true, "favoriteNumbers": [1, 2, 3], "address": {"street": "123 Main St", "city": "New York"}}
"""

if let data = jsonString.data(using: .utf8) {
    do {
        if let json = try JSONSerialization.jsonObject(with: data, options: []) as? [String: Any] {
            let name = json["name"] as? String
            let age = json["age"] as? Int
            let isEmployed = json["isEmployed"] as? Bool
            let favoriteNumbers = json["favoriteNumbers"] as? [Int]
            
            if let addressDict = json["address"] as? [String: Any] {
                let street = addressDict["street"] as? String
                let city = addressDict["city"] as? String
                
                // Use the extracted values
                print("Name: \(name ?? "")")
                print("Age: \(age ?? 0)")
                print("Is Employed: \(isEmployed ?? false)")
                print("Favorite Numbers: \(favoriteNumbers ?? [])")
                print("Address: \(street ?? ""), \(city ?? "")")
            }
        }
    } catch {
        print("Error parsing JSON: \(error.localizedDescription)")
    }
}
```

Here, the `JSONSerialization` class helps parse the JSON data manually by casting values to the appropriate data types.

## Conclusion

Parsing JSON with different data types is a crucial aspect of working with APIs and external data in Swift. Using the `Codable` protocol or `JSONSerialization`, you can easily decode JSON into custom model types or manually extract values from the JSON object. Choose the approach that best suits your project and enjoy efficient JSON parsing in Swift!

#swift #JSON