---
layout: post
title: "Applying Encoding/Decoding Functions in Swift"
description: " "
date: 2023-09-29
tags: [swift, encodindecoding]
comments: true
share: true
---

Swift provides built-in functions for encoding and decoding data, making it easier to convert between different formats such as JSON, Property List, or custom formats. In this blog post, we will explore how to use Swift's encoding and decoding functions to work with different data formats.

## Encoding Data

Encoding refers to converting a data object into a specific format. Swift provides the `JSONEncoder` and `PropertyListEncoder` classes to encode data objects into JSON and Property List formats respectively. Let's take a look at an example of encoding a custom struct into JSON format:

```swift
struct Person: Codable {
    var name: String
    var age: Int
    var profession: String
}

let person = Person(name: "John Doe", age: 30, profession: "Engineer")

let encoder = JSONEncoder()
do {
    let encodedData = try encoder.encode(person)
    let jsonString = String(data: encodedData, encoding: .utf8)
    print(jsonString ?? "")
} catch {
    print("Error encoding data: \(error.localizedDescription)")
}
```

In the above code, we define a `Person` struct conforming to the `Codable` protocol, which indicates that the struct can be encoded and decoded. We create an instance of the `Person` struct and then use `JSONEncoder` to encode it into JSON format. Finally, we print the encoded data in string format.

## Decoding Data

Decoding refers to converting data from a specific format back into objects. Swift provides the `JSONDecoder` and `PropertyListDecoder` classes to decode data from JSON and Property List formats respectively. Here's an example of decoding a JSON string into a custom struct:

```swift
let jsonString = """
{
    "name": "John Doe",
    "age": 30,
    "profession": "Engineer"
}
"""

let decoder = JSONDecoder()
if let jsonData = jsonString.data(using: .utf8) {
    do {
        let decodedPerson = try decoder.decode(Person.self, from: jsonData)
        print(decodedPerson)
    } catch {
        print("Error decoding data: \(error.localizedDescription)")
    }
}
```

In the code above, we have a JSON string representing a `Person` object. We use `JSONDecoder` to decode the JSON string into an instance of the `Person` struct. The `decode` method takes two parameters: the type of the object to decode (in this case, `Person.self`), and the data to decode.

## Conclusion

Swift's encoding and decoding functions provide a convenient way to work with different data formats. Whether you need to encode data into JSON, Property List, or any other format, or decode data back into objects, Swift's built-in functions make the process simple and efficient.

#swift #encodindecoding