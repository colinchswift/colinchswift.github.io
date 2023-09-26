---
layout: post
title: "Decoding JSON into Swift classes"
description: " "
date: 2023-09-27
tags: [Swift, JSONDecoding]
comments: true
share: true
---

To decode JSON into Swift classes, follow these steps:

1. Define your Swift class or struct to match the structure of the JSON data. Make sure to adopt the Codable protocol.

```swift
struct Book: Codable {
    let title: String
    let author: String
    let publicationYear: Int
}
```

2. Get the JSON data from an API response or any other source.

```swift
let jsonData = """
{
    "title": "The Great Gatsby",
    "author": "F. Scott Fitzgerald",
    "publicationYear": 1925
}
""".data(using: .utf8)!
```

3. Create an instance of JSONDecoder and use it to decode the JSON data into your Swift class.

```swift
let decoder = JSONDecoder()
do {
    let book = try decoder.decode(Book.self, from: jsonData)
    print(book.title) // Output: The Great Gatsby
    print(book.author) // Output: F. Scott Fitzgerald
    print(book.publicationYear) // Output: 1925
} catch {
    print("Error: \(error)")
}
```

In the code above, the JSONDecoder decodes the jsonData into an instance of the Book struct.

4. Handle any decoding errors that may occur. The decoding process can throw an error if the JSON data is not valid or does not match the structure of the Swift class.

The Codable protocol provides default implementations for many commonly used types like String, Int, Date, etc. If your Swift class contains nested objects or arrays, the nested types must also adopt the Codable protocol.

By using the Codable protocol, decoding JSON into Swift classes becomes a straightforward process. It simplifies working with JSON data in Swift and makes your code more readable and maintainable.

#Swift #JSONDecoding