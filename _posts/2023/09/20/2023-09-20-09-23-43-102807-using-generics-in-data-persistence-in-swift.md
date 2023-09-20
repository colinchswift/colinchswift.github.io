---
layout: post
title: "Using generics in data persistence in Swift"
description: " "
date: 2023-09-20
tags: [SwiftProgramming]
comments: true
share: true
---

Data persistence is a crucial aspect of modern app development, allowing us to store and retrieve data even after the app is closed. In Swift, generics are a powerful tool that can be leveraged to make our code more flexible and reusable. In this blog post, we will explore how to use generics to achieve data persistence in Swift.

## What are Generics?

Generics in Swift allow us to write flexible and reusable code. With generics, we can define functions, classes, and structures that can work with any data type, rather than being tied to a specific type. This is particularly useful in scenarios where we want to write code that can handle different data types but avoids duplication.

## Creating a Generic Data Persistence Manager

To start, let's create a generic data persistence manager that can handle different types of data. We'll use the `Codable` protocol, which provides encoding and decoding functionality, to store and retrieve data from the disk.

First, let's define our `PersistenceManager` class:

```swift
final class PersistenceManager<T: Codable> {
    private let fileManager = FileManager.default
    private let encoder = JSONEncoder()
    private let decoder = JSONDecoder()

    func saveData(_ data: T, to fileURL: URL) throws {
        let encodedData = try encoder.encode(data)
        try encodedData.write(to: fileURL)
    }

    func loadData(from fileURL: URL) throws -> T {
        let fileData = try Data(contentsOf: fileURL)
        let decodedData = try decoder.decode(T.self, from: fileData)
        return decodedData
    }
}
```

In the `PersistenceManager` class, we have defined two methods: `saveData` and `loadData`. The generic type `T` represents the type of data we want to store or retrieve.

## Using the Generic Data Persistence Manager

Now that we have our generic data persistence manager, let's see how we can use it to store and retrieve data. We'll be using a `User` struct as an example data type:

```swift
struct User: Codable {
    let name: String
    let age: Int
}
```

To save a `User` object:

```swift
let user = User(name: "John Doe", age: 25)
let persistenceManager = PersistenceManager<User>()
do {
    let documentsURL = fileManager.urls(for: .documentDirectory, in: .userDomainMask).first!
    let fileURL = documentsURL.appendingPathComponent("user.json")
    try persistenceManager.saveData(user, to: fileURL)
    // Data saved successfully
} catch {
    // Handle the error
}
```

To load the saved `User` object:

```swift
do {
    let documentsURL = fileManager.urls(for: .documentDirectory, in: .userDomainMask).first!
    let fileURL = documentsURL.appendingPathComponent("user.json")
    let loadedUser = try persistenceManager.loadData(from: fileURL)
    // Use the loadedUser object
} catch {
    // Handle the error
}
```

## Conclusion

Using generics in data persistence allows us to create flexible and reusable code, as it enables us to handle different types of data without duplication. By leveraging the power of generics and the `Codable` protocol in Swift, we can achieve efficient and type-safe data persistence in our applications.

#iOS #SwiftProgramming