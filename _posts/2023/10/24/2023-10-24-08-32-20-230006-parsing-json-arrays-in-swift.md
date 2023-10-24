---
layout: post
title: "Parsing JSON arrays in Swift"
description: " "
date: 2023-10-24
tags: []
comments: true
share: true
---

In Swift, parsing JSON arrays can be done easily using the `Codable` protocol and the `JSONDecoder` class provided by the Foundation framework. This allows for a more efficient and convenient way to parse and work with JSON data in your Swift applications.

## 1. Define your data model

Start by defining a struct that represents the structure of the data you want to parse from the JSON array. This struct should conform to the `Codable` protocol.

```swift
struct Person: Codable {
    let name: String
    let age: Int
}
```

## 2. Parse the JSON array

Assuming you have a JSON array represented as a `Data` object, you can parse it using the `JSONDecoder` class. Here's an example of how you can parse the JSON array into an array of `Person` objects:

```swift
let json = """
[
    {"name": "John", "age": 25},
    {"name": "Jane", "age": 30}
]
"""

do {
    let jsonData = json.data(using: .utf8)!
    let people = try JSONDecoder().decode([Person].self, from: jsonData)
    
    for person in people {
        print("Name: \(person.name), Age: \(person.age)")
    }
} catch {
    print("Error parsing JSON: \(error)")
}
```

## 3. Access the parsed data

Once the JSON array is successfully parsed, you can access the individual elements like any other Swift array. In the example above, we iterate over the `people` array and print each person's name and age.

## Conclusion

By utilizing the `Codable` protocol and the `JSONDecoder` class in Swift, parsing JSON arrays becomes a straightforward process. This allows for seamless integration with JSON-based APIs and makes working with JSON data in Swift applications much more convenient.

#References

- [Swift Codable documentation](https://developer.apple.com/documentation/swift/codable)
- [Foundation Framework documentation](https://developer.apple.com/documentation/foundation)