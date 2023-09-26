---
layout: post
title: "Handling errors when encoding and decoding with Codable"
description: " "
date: 2023-09-22
tags: [Codable]
comments: true
share: true
---

In Swift, errors thrown during encoding or decoding can be caught using do-catch blocks. This allows you to handle specific errors and provide appropriate error messages to the user.

Let's take a look at an example where encoding and decoding can throw errors:

```swift
struct Person: Codable {
    var name: String
    var age: Int
}

let jsonString = """
    {
        "name": "John Doe",
        "age": "twenty-five"
    }
"""

if let jsonData = jsonString.data(using: .utf8) {
    do {
        let person = try JSONDecoder().decode(Person.self, from: jsonData)
        print("Decoded person: \(person)")
    } catch let error {
        print("Error decoding person: \(error.localizedDescription)")
    }
} else {
    print("Invalid JSON string")
}
```

In this example, we have a `Person` struct that conforms to the `Codable` protocol. The JSON string contains an age value as a string instead of an integer. When attempting to decode the JSON into a `Person` instance, an error will be thrown.

Inside the catch block, we can access the error using the `error` parameter. We can use `localizedDescription` to get a human-readable description of the error. This helps in diagnosing the issue during development or providing feedback to the user in a production app.

Similarly, we can handle errors during encoding:

```swift
let person = Person(name: "Jane Smith", age: 30)

do {
    let jsonData = try JSONEncoder().encode(person)
    let jsonString = String(data: jsonData, encoding: .utf8)
    print("Encoded JSON: \(jsonString ?? "")")
} catch let error {
    print("Error encoding person: \(error.localizedDescription)")
}
```

In this case, if there are any issues during encoding, the error will be caught in the catch block. We can then provide appropriate error handling based on the specific error encountered.

By properly handling errors during encoding and decoding with Codable, we can ensure our application gracefully handles unexpected data or formatting issues, improving the reliability and usability of our code.

#Swift #Codable