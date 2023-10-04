---
layout: post
title: "Using async/await with Codable in Swift"
description: " "
date: 2023-10-04
tags: [introduction), async]
comments: true
share: true
---

## Table of Contents
- [Introduction](#introduction)
- [Async/Await in Swift](#async-await-in-swift)
- [Combining Codable with Async/Await](#combining-codable-with-async-await)
- [Conclusion](#conclusion)

## Introduction
Swift provides powerful tools for handling asynchronous code, such as `async` and `await`. Codable is a feature introduced in Swift 4, which allows you to easily encode and decode data structures to and from JSON or other formats. In this blog post, we will explore how to combine `async`/`await` with Codable in Swift, to make working with asynchronous API calls and JSON parsing more streamlined.

## Async/Await in Swift
Async/await is a language feature that allows you to write asynchronous code in a more synchronous-like manner. By using the `async` keyword, you can define a function that performs an asynchronous task, and by using `await`, you can pause the execution of the function until the asynchronous task is completed. This makes the code more readable and avoids callback hell.

## Combining Codable with Async/Await
To combine Codable with async/await, you can use the `URLSession` class to make API calls and then decode the JSON response using the `JSONDecoder`. Here's an example of how you can do this:

```swift
struct User: Codable {
    let id: Int
    let name: String
    let email: String
}

enum APIError: Error {
    case invalidURL
    case networkError
    case decodingError
}

func fetchData() async throws -> [User] {
    guard let url = URL(string: "https://api.example.com/users") else {
        throw APIError.invalidURL
    }
    
    do {
        let (data, _) = try await URLSession.shared.data(from: url)
        let users = try JSONDecoder().decode([User].self, from: data)
        return users
    } catch {
        throw APIError.decodingError
    }
}

async {
    do {
        let users = try await fetchData()
        print(users)
    } catch {
        print("Error: \(error)")
    }
}
```

In the above code, we define a `User` struct which conforms to the `Codable` protocol. We then define an `enum` called `APIError` to handle any errors that may occur during the API call or JSON decoding.

The `fetchData()` function is declared as `async throws` to indicate that it performs an asynchronous task and can throw an error. Inside this function, we first check if the URL is valid, then make an API call using `URLSession.shared.data(from:)`. We use `await` to wait for the data to be retrieved and then decode it using `JSONDecoder`. If any error occurs during the process, we throw an `APIError.decodingError`.

Finally, we call `fetchData()` using `await` inside an `async` block and handle any errors that may occur.

## Conclusion
Using async/await with Codable in Swift provides a more streamlined and readable way to handle asynchronous API calls and JSON parsing. With the power of Swift's async/await syntax and the convenience of Codable, handling asynchronous operations becomes much more intuitive and concise.