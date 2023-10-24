---
layout: post
title: "Combining JSON from different sources in Swift"
description: " "
date: 2023-10-24
tags: [JSON]
comments: true
share: true
---

When working with APIs, it is common to receive JSON data from different sources and need to combine them into a single JSON object in Swift. This can be done easily using the `Codable` protocol provided by the Swift standard library.

Let's imagine we have two JSON strings representing user information:

```swift
let jsonString1 = """
{
    "name": "John Doe",
    "age": 25
}
"""

let jsonString2 = """
{
    "address": "123 Main St",
    "city": "New York"
}
```

To combine these two JSON strings into a single JSON object, we need to create Swift structs that conform to the `Codable` protocol and then use the `JSONDecoder` and `JSONEncoder` to decode and encode the JSON strings.

First, let's define our structs:

```swift
struct User: Codable {
    let name: String
    let age: Int
}

struct Address: Codable {
    let address: String
    let city: String
}
```

Next, we can create instances of the structs using the JSON strings:

```swift
let decoder = JSONDecoder()

guard let jsonData1 = jsonString1.data(using: .utf8),
      let user = try? decoder.decode(User.self, from: jsonData1) else {
    fatalError("Failed to decode JSON data")
}

guard let jsonData2 = jsonString2.data(using: .utf8),
      let address = try? decoder.decode(Address.self, from: jsonData2) else {
    fatalError("Failed to decode JSON data")
}
```

Now that we have the decoded objects, we can combine them into a single JSON object:

```swift
let combinedJSON = try JSONEncoder().encode(CombinedUser(user: user, address: address))
let combinedString = String(data: combinedJSON, encoding: .utf8)
```

In this example, `CombinedUser` is a new struct that includes both the `User` and `Address` information:

```swift
struct CombinedUser: Codable {
    let user: User
    let address: Address
}
```

Finally, we can print the combined JSON string:

```swift
if let combinedString = combinedString {
    print(combinedString)
}
```

This will output the following JSON string:

```plaintext
{
    "user": {
        "name": "John Doe",
        "age": 25
    },
    "address": {
        "address": "123 Main St",
        "city": "New York"
    }
}
```

By combining the JSON data from different sources using the `Codable` protocol and `JSONDecoder/JSONEncoder`, we can easily work with JSON objects in Swift.

## Conclusion

Combining JSON from different sources in Swift can be achieved by decoding the JSON data into separate structs and then combining them into a single struct that conforms to the `Codable` protocol. The `JSONDecoder` and `JSONEncoder` provide convenient ways to decode and encode JSON data in Swift. 

#swift #JSON