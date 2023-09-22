---
layout: post
title: "Converting Swift objects to JSON using Codable"
description: " "
date: 2023-09-22
tags: [Swift, JSON]
comments: true
share: true
---

In Swift, you can easily convert Swift objects to JSON using the `Codable` protocol. `Codable` allows you to encode and decode Swift objects into various formats, including JSON, with just a few lines of code.

Here's how you can convert a Swift object to JSON using `Codable`:

## Step 1: Define the Model

First, you need to define your Swift object model by conforming it to the `Codable` protocol. Let's take an example of a `Person` struct with `name` and `age` properties:

```swift
struct Person: Codable {
    let name: String
    let age: Int
}
```

## Step 2: Encoding the Object

To convert the Swift object to JSON, you can use the `JSONEncoder` class:

```swift
let person = Person(name: "John Doe", age: 30)
let encoder = JSONEncoder()
encoder.outputFormatting = .prettyPrinted

do {
    let jsonData = try encoder.encode(person)
    if let jsonString = String(data: jsonData, encoding: .utf8) {
        print(jsonString)
    }
} catch {
    print("Encoding failed: \(error.localizedDescription)")
}
```
In the above code, we create a `JSONEncoder` instance and set its `outputFormatting` property to `.prettyPrinted` to make the output JSON more readable. We then use the `encode` method to convert the Swift object into JSON data. Finally, we convert the JSON data to a string using `String(data:encoding:)` and print it.

The output would be:

```json
{
  "name" : "John Doe",
  "age" : 30
}
```

## Step 3: Decoding the JSON

If you want to convert the JSON back into a Swift object, you can use the `JSONDecoder` class:

```swift
let jsonString = """
{
    "name": "John Doe",
    "age": 30
}
"""

let decoder = JSONDecoder()

do {
    let jsonData = jsonString.data(using: .utf8)
    let person = try decoder.decode(Person.self, from: jsonData!)
    print(person)
} catch {
    print("Decoding failed: \(error.localizedDescription)")
}
```

In this code, we create a `JSONDecoder` instance and use the `decode` method to convert the JSON data back into a Swift object of type `Person`. 

The output would be:

```
Person(name: "John Doe", age: 30)
```

By using `Codable`, you can easily convert Swift objects to JSON and vice versa, saving you time and effort when working with JSON data in Swift applications. 

Remember to import `Foundation` to use `JSONEncoder` and `JSONDecoder`.