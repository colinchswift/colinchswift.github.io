---
layout: post
title: "Using CodingKeys to control the encoding and decoding process in Codable"
description: " "
date: 2023-09-22
tags: [swift, codable]
comments: true
share: true
---

In Swift, the `Codable` protocol provides a convenient way to convert custom types to and from external representations, such as JSON or property list. It automatically handles most of the encoding and decoding process for you. However, there may be scenarios where you want more control over the naming and structure of the encoded and decoded data.

One way to achieve this is by using the `CodingKeys` enum. By defining a `CodingKeys` enum within your custom type, you can specify custom names for properties during the encoding and decoding process.

Here's an example:

```swift
struct Person: Codable {
    let firstName: String
    let lastName: String
    let age: Int

    // CodingKeys enum to specify custom names
    enum CodingKeys: String, CodingKey {
        case firstName = "first_name"
        case lastName = "last_name"
        case age
    }
}
```

In the above example, we have a `Person` struct that conforms to the `Codable` protocol. By default, the property names will be used to encode and decode the data. However, with the `CodingKeys` enum, we can provide custom names for properties during the encoding and decoding process.

In this case, we are specifying that the `firstName` property should be encoded as "first_name" and the `lastName` property should be encoded as "last_name". The `age` property, on the other hand, will retain its original name.

Now, let's encode an instance of the `Person` struct:

```swift
let person = Person(firstName: "John", lastName: "Doe", age: 30)

let encoder = JSONEncoder()
encoder.outputFormatting = .prettyPrinted

do {
    let encodedData = try encoder.encode(person)
    let jsonString = String(data: encodedData, encoding: .utf8)
    print(jsonString ?? "")
} catch {
    print("Encoding failed: \(error)")
}
```

The output will be:

```json
{
  "first_name" : "John",
  "last_name" : "Doe",
  "age" : 30
}
```

As you can see, the property names are encoded according to the values specified in the `CodingKeys` enum.

Similarly, when decoding data, the `CodingKeys` enum will be used to map the encoded keys to the appropriate properties:

```swift
let json = """
{
  "first_name" : "Jane",
  "last_name" : "Smith",
  "age" : 25
}
"""

let decoder = JSONDecoder()

do {
    let decodedPerson = try decoder.decode(Person.self, from: json.data(using: .utf8)!)
    print(decodedPerson)
} catch {
    print("Decoding failed: \(error)")
}
```

The output will be:

```
Person(firstName: "Jane", lastName: "Smith", age: 25)
```

By using the `CodingKeys` enum, you have complete control over the encoding and decoding process of your custom types. It allows you to customize the naming and structure of your data representations, making it easier to work with external data formats while maintaining consistency within your codebase.

#swift #codable