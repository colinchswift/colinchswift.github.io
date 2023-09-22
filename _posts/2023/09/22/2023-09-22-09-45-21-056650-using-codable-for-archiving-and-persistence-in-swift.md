---
layout: post
title: "Using Codable for archiving and persistence in Swift"
description: " "
date: 2023-09-22
tags: [Swift, Codable]
comments: true
share: true
---

With the introduction of `Codable` in Swift, archiving and persistence of data has become easier than ever. `Codable` provides a simple way to encode and decode Swift types to and from various external representations, such as JSON, binary, or property list.

In this blog post, we will explore how to utilize `Codable` for archiving and persistence in Swift applications.

## Archiving Objects

`Codable` allows us to encode Swift objects into a binary representation that can be stored in a file or sent over the network. To archive an object, you need to ensure that the class or struct conforms to the `Codable` protocol.

Here's an example of a simple `User` struct that can be archived:

```swift
struct User: Codable {
    var name: String
    var age: Int
}
```

To archive an instance of the `User` struct, you can use the `JSONEncoder`:

```swift
let user = User(name: "John Doe", age: 30)
let encoder = JSONEncoder()

do {
    let data = try encoder.encode(user)
    // Save the data to a file or send it over the network
} catch {
    print("Failed to encode user: \(error)")
}
```

## Restoring Objects

Once an object has been archived, we can easily restore it back to its original state using the `JSONDecoder`:

```swift
let decoder = JSONDecoder()

do {
    let restoredUser = try decoder.decode(User.self, from: data)
    print(restoredUser)
} catch {
    print("Failed to decode user: \(error)")
}
```

## Persisting Data

Archiving and restoring objects is useful, but often we want to persist the data for later use. `Codable` makes this process seamless by allowing us to easily save and load the encoded data to and from the disk.

Here's an example of saving and loading the `User` object to a file:

```swift
let documentsDirectory = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask).first!
let fileURL = documentsDirectory.appendingPathComponent("user.json")

do {
    let data = try encoder.encode(user)
    try data.write(to: fileURL)
} catch {
    print("Failed to save user: \(error)")
}

do {
    let loadedData = try Data(contentsOf: fileURL)
    let restoredUser = try decoder.decode(User.self, from: loadedData)
    print(restoredUser)
} catch {
    print("Failed to load user: \(error)")
}
```

By using `Codable` and the `JSONEncoder`/`JSONDecoder`, we can easily archive and persist Swift objects with just a few lines of code.

#Swift #Codable