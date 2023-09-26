---
layout: post
title: "Decoding JSON with URL values in Swift"
description: " "
date: 2023-09-27
tags: [JSON, Swift]
comments: true
share: true
---

When working with JSON in Swift, it's common to encounter URL values that need to be decoded properly. In this blog post, we'll explore how to decode JSON with URL values and handle any potential errors.

## 1. Define the model structure

First, you need to define the model structure that matches the JSON you're decoding. Suppose we have the following JSON data:

```json
{
  "title": "Example",
  "url": "https://www.example.com"
}
```

To decode this JSON, create a struct that conforms to the `Decodable` protocol:

```swift
struct Article: Decodable {
    let title: String
    let url: URL
}
```

Notice that we use the `URL` type for the `url` property.

## 2. Decode the JSON using `JSONDecoder`

To decode the JSON with URL values, create an instance of `JSONDecoder` and set its `dateDecodingStrategy` property to `.iso8601`. This ensures that the URL string value is properly decoded into a `URL` object.

```swift
let jsonString = """
{
  "title": "Example",
  "url": "https://www.example.com"
}
"""

let jsonData = jsonString.data(using: .utf8)!
let decoder = JSONDecoder()
decoder.dateDecodingStrategy = .iso8601

do {
    let article = try decoder.decode(Article.self, from: jsonData)
    print(article.title)
    print(article.url)
} catch {
    print("Error decoding JSON: \(error)")
}
```

## 3. Handling decoding errors

If the JSON contains an invalid URL string, the decoding will throw an error. To handle this, you can catch the error and display a suitable message to the user.

```swift
do {
    let article = try decoder.decode(Article.self, from: jsonData)
    print(article.title)
    print(article.url)
} catch {
    if let decodingError = error as? DecodingError {
        switch decodingError {
            case .dataCorrupted(let context):
                print("Data corrupted: \(context.debugDescription)")
            case .typeMismatch(_, let context):
                print("Type mismatch: \(context.debugDescription)")
            case .valueNotFound(_, let context):
                print("Value not found: \(context.debugDescription)")
            case .keyNotFound(_, let context):
                print("Key not found: \(context.debugDescription)")
            @unknown default:
                print("Unknown decoding error")
        }
    } else {
        print("Error decoding JSON: \(error)")
    }
}
```

By handling the decoding errors properly, you ensure that your app gracefully handles any JSON with invalid URL values.

## Conclusion

Decoding JSON with URL values in Swift is straightforward once you define the correct model structure and handle any potential errors. Use the `URL` type for properties that represent URL values and handle decoding errors to ensure smooth JSON parsing in your app.

#JSON #Swift