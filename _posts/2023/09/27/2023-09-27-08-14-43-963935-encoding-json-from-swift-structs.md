---
layout: post
title: "Encoding JSON from Swift structs"
description: " "
date: 2023-09-27
tags: [JSONEncoding]
comments: true
share: true
---

In Swift, converting a struct or an object into JSON format is a common requirement when working with APIs or storing data. Fortunately, the `Codable` protocol provides a convenient way to encode and decode JSON data.

Here's an example of how to encode a Swift struct as JSON using the `Codable` protocol:

1. Define a struct that conforms to the `Codable` protocol. Suppose we have a `Person` struct with a name and age property:

```swift
struct Person: Codable {
    let name: String
    let age: Int
}
```

2. Create an instance of the struct:

```swift
let person = Person(name: "John Doe", age: 30)
```

3. Use a JSONEncoder instance to encode the struct into JSON data:

```swift
let encoder = JSONEncoder()
do {
    let jsonData = try encoder.encode(person)
    if let jsonString = String(data: jsonData, encoding: .utf8) {
        print(jsonString)
    }
} catch {
    print("Error encoding person: \(error)")
}
```

In the above code, we create a `JSONEncoder` instance to perform the encoding. The `encode()` method is used to encode the `person` struct into `jsonData`. We then convert the `jsonData` into a string representation using the `.utf8` encoding.

4. The output string representing the JSON data will be:

```json
{"name":"John Doe","age":30}
```

With the above steps, we successfully encoded the Swift struct `Person` into JSON format using the `Codable` protocol.

Remember to handle any potential errors that may occur during the encoding process. The `try` keyword is used to catch any encoding errors that throw an exception.

By following the above method, you can easily encode any Swift structs or objects into JSON format for further processing or transmission.

#Swift #JSONEncoding