---
layout: post
title: "Using JSONEncoder and JSONDecoder with web APIs in Swift"
description: " "
date: 2023-09-27
tags: [JSON]
comments: true
share: true
---

In modern app development, integrating web APIs is a common task. When working with APIs, data is often exchanged in JSON format. In Swift, the Codable protocol combined with the JSONEncoder and JSONDecoder classes provides a powerful and convenient way to serialize and deserialize JSON data.

## Encoding JSON

To encode an object into JSON, we'll use the JSONEncoder class. This class converts Swift objects conforming to the Codable protocol into a JSON Data object.

Here's an example of encoding an object into JSON:

```swift
struct User: Codable {
    let name: String
    let email: String
}

let user = User(name: "John Doe", email: "johndoe@example.com")

let encoder = JSONEncoder()
encoder.outputFormatting = .prettyPrinted

do {
    let jsonData = try encoder.encode(user)
    if let jsonString = String(data: jsonData, encoding: .utf8) {
        print(jsonString)
    }
} catch {
    print("Error encoding JSON: \(error)")
}

```

In this example, we have a simple User struct conforming to the Codable protocol. We create an instance of User, and then instantiate a JSONEncoder. We set the outputFormatting property to .prettyPrinted for a readable JSON output. The encode() method is used to convert the User object into JSON Data. The resulting data is then converted to a JSON string using the String initializer.

## Decoding JSON

To decode JSON data into an object, we'll use the JSONDecoder class. The JSONDecoder class helps to convert JSON Data into Swift objects conforming to the Codable protocol.

Here's an example of decoding JSON into an object:

```swift
let jsonString = """
{
    "name": "John Doe",
    "email": "johndoe@example.com"
}
"""

let jsonData = jsonString.data(using: .utf8)

let decoder = JSONDecoder()

do {
    let user = try decoder.decode(User.self, from: jsonData!)
    print(user)
} catch {
    print("Error decoding JSON: \(error)")
}
```

In this example, we have a JSON string representing a User object. We convert the JSON string into JSON Data using the data(using: .utf8) method. Then, we create a JSONDecoder instance and use the decode() method to convert the JSON Data into a User object. By specifying the type as User.self, the decoder knows what type of object it should create.

## Conclusion

With the help of JSONEncoder and JSONDecoder, integrating and working with web APIs in Swift becomes much easier. The Codable protocol simplifies the process of serializing and deserializing JSON data, saving developers time and effort. It's an essential tool to work effectively with web APIs in modern Swift app development.

#Swift #JSON #Coding