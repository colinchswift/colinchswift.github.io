---
layout: post
title: "Parsing JSON data in Swift scripts"
description: " "
date: 2023-10-06
tags: [ID299]
comments: true
share: true
---

When working with Swift scripts, you might encounter scenarios where you need to parse JSON data. JSON (JavaScript Object Notation) is a popular data interchange format used to represent structured data. In this blog post, we will explore how to parse JSON data in Swift scripts.

## Table of Contents

- [What is JSON?](#what-is-json)
- [Parsing JSON in Swift](#parsing-json-in-swift)
  - [Decodable](#decodable)
  - [Manually parsing](#manually-parsing)
- [Conclusion](#conclusion)

## What is JSON?

JSON is a lightweight data interchange format that is easy for humans to read and write. It consists of key-value pairs and supports various data types such as numbers, strings, booleans, arrays, and objects. JSON is widely used for transmitting data between a server and a web application, but it can also be used in scripting languages like Swift.

## Parsing JSON in Swift

Swift provides convenient ways to parse JSON data. Here are two common approaches to parsing JSON in Swift.

### Decodable

Swift's `Decodable` protocol allows for easy parsing of JSON data into Swift structs or classes. By conforming to the `Decodable` protocol, you can define a mapping between the JSON data and your Swift data structure.

```swift
struct Person: Decodable {
    let name: String
    let age: Int
}

let jsonString = """
{
    "name": "John Doe",
    "age": 25
}
"""

if let jsonData = jsonString.data(using: .utf8) {
    let decoder = JSONDecoder()
    do {
        let person = try decoder.decode(Person.self, from: jsonData)
        print("Name: \(person.name), Age: \(person.age)")
    } catch {
        print("Error decoding JSON: \(error.localizedDescription)")
    }
}
```

In the above example, we define a `Person` struct and conform it to the `Decodable` protocol. We then use `JSONDecoder` to decode the JSON data into an instance of `Person`.

### Manually parsing

If you prefer more control over the parsing process, you can manually parse JSON using Swift's `JSONSerialization` class. `JSONSerialization` provides methods to convert JSON data into Swift objects such as arrays and dictionaries.

```swift
let jsonString = """
{
    "name": "John Doe",
    "age": 25
}
"""

if let jsonData = jsonString.data(using: .utf8) {
    do {
        let jsonObject = try JSONSerialization.jsonObject(with: jsonData, options: [])
        if let jsonDictionary = jsonObject as? [String: Any] {
            if let name = jsonDictionary["name"] as? String, let age = jsonDictionary["age"] as? Int {
                print("Name: \(name), Age: \(age)")
            }
        }
    } catch {
        print("Error parsing JSON: \(error.localizedDescription)")
    }
}
```

In the above example, we convert the JSON data into a generic `Any` object using `JSONSerialization`. We then check if it's a dictionary and extract the values for the "name" and "age" keys manually.

## Conclusion

Parsing JSON data in Swift scripts is a common task when working with external APIs or reading data from files. Swift provides convenient ways to parse JSON using the `Decodable` protocol or `JSONSerialization` class. Understanding how to parse JSON data enables you to work with external data sources more effectively in your Swift scripts.

For more information about Swift and JSON parsing, refer to the official [Swift documentation](https://docs.swift.org/swift-book/LanguageGuide/StringsAndCharacters.html#ID299).