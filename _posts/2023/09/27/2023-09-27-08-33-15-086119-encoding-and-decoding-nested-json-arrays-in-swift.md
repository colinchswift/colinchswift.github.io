---
layout: post
title: "Encoding and decoding nested JSON arrays in Swift"
description: " "
date: 2023-09-27
tags: [Swift, JSON]
comments: true
share: true
---

JSON (JavaScript Object Notation) is a popular data interchange format used for sending and receiving data between different systems. In Swift, working with JSON is made easy with the `Codable` protocol, which allows for seamless encoding and decoding of data structures.

However, when dealing with nested JSON arrays, developers may face some challenges. In this article, we will explore how to encode and decode nested JSON arrays in Swift.

## Encoding Nested JSON Arrays

To encode a nested JSON array, we need to ensure that the data structure we want to encode conforms to the `Encodable` protocol. Let's consider an example where we have a data structure representing a bookstore containing multiple books, each having multiple authors.

```swift
struct Book: Codable {
    let title: String
    let authors: [String]
}

struct Bookstore: Codable {
    let name: String
    let books: [Book]
}
```

In the above example, the `Book` struct represents a book with a `title` and an array of `authors`, while the `Bookstore` struct represents a bookstore with a `name` and an array of `books`.

To encode the `Bookstore` object, we can use the `JSONEncoder` class:

```swift
let bookstore = Bookstore(name: "My Bookstore", books: [book1, book2, book3])

let encoder = JSONEncoder()
encoder.outputFormatting = .prettyPrinted

if let jsonData = try? encoder.encode(bookstore),
   let jsonString = String(data: jsonData, encoding: .utf8) {
    print(jsonString)
}
```

The above code demonstrates how to encode a `Bookstore` object and convert it to a JSON string. By setting `outputFormatting` to `.prettyPrinted`, we get a nicely formatted JSON string.

## Decoding Nested JSON Arrays

Decoding nested JSON arrays follows a similar pattern. We need to ensure that our data structure conforms to the `Decodable` protocol.

Continuing with the previous example, let's see how we can decode a JSON string representing a `Bookstore` object:

```swift
let jsonString = """
{
    "name": "My Bookstore",
    "books": [
        {
            "title": "Book 1",
            "authors": ["Author 1", "Author 2"]
        },
        {
            "title": "Book 2",
            "authors": ["Author 3", "Author 4"]
        },
        {
            "title": "Book 3",
            "authors": ["Author 5"]
        }
    ]
}
"""

let decoder = JSONDecoder()

if let jsonData = jsonString.data(using: .utf8),
   let bookstore = try? decoder.decode(Bookstore.self, from: jsonData) {
    print(bookstore.name)
    for book in bookstore.books {
        print("\(book.title) by \(book.authors.joined(separator: ", "))")
    }
}
```

The above code demonstrates how to decode a JSON string representing a `Bookstore` object using the `JSONDecoder` class. The resulting `bookstore` object will contain the decoded data, which we can access and process as needed.

## Conclusion

Encoding and decoding nested JSON arrays in Swift is a straightforward process when utilizing the `Codable` protocol. By ensuring that our data structures conform to the `Encodable` and `Decodable` protocols, we can easily convert our data to JSON and back. This makes working with JSON data in Swift a breeze, even for complex nested arrays.

#Swift #JSON