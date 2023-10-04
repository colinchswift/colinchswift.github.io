---
layout: post
title: "Decoding JSON with nested arrays in Swift"
description: " "
date: 2023-09-27
tags: [JSON]
comments: true
share: true
---

When working with JSON data in Swift, it's common to encounter nested arrays, where an array is nested within another array. In this blog post, we'll learn how to decode JSON with nested arrays using Swift's Codable protocol.

## JSON Structure

Let's consider an example JSON structure that contains nested arrays:

```swift
{
    "name": "John Doe",
    "age": 25,
    "hobbies": ["reading", "writing", "coding"],
    "friends": [
        {
            "name": "Jane Smith",
            "age": 27,
            "hobbies": ["painting", "singing"]
        },
        {
            "name": "John Smith",
            "age": 26,
            "hobbies": ["swimming", "traveling"]
        }
    ]
}
```

The "hobbies" key contains a nested array, and the "friends" key contains an array of objects, each with their own nested array of hobbies.

## Creating Model Structures

To decode this JSON structure, we need to create corresponding model structures in Swift. Let's start with the `Person` model:

```swift
struct Person: Codable {
    let name: String
    let age: Int
    let hobbies: [String]
    let friends: [Person]
}
```

As you can see, the `Person` struct has properties for the name, age, hobbies, and friends. The `friends` property is of the same type `Person`, allowing us to nest one `Person` inside another.

## Decoding Nested Arrays

To decode the JSON data with nested arrays, we can use the `JSONDecoder` class provided by Swift's Foundation framework. Here's how we can decode the JSON:

```swift
let jsonString = """
{
    "name": "John Doe",
    "age": 25,
    "hobbies": ["reading", "writing", "coding"],
    "friends": [
        {
            "name": "Jane Smith",
            "age": 27,
            "hobbies": ["painting", "singing"]
        },
        {
            "name": "John Smith",
            "age": 26,
            "hobbies": ["swimming", "traveling"]
        }
    ]
}
"""

let jsonData = jsonString.data(using: .utf8)

let decoder = JSONDecoder()

do {
    let person = try decoder.decode(Person.self, from: jsonData!)
    print(person)
} catch {
    print("Error decoding JSON: \(error)")
}
```

In this example, we create a `JSONDecoder` instance and use the `decode(_:from:)` method to decode the JSON data into an instance of the `Person` model.

## Conclusion

In this blog post, we've learned how to decode JSON with nested arrays using Swift's Codable protocol. By creating model structures that reflect the JSON structure, we can easily decode and work with nested arrays in our Swift applications.

Remember, when working with nested arrays, it's important to ensure that your model structures accurately reflect the JSON structure to ensure successful decoding.

#Swift #JSON #NestedArrays