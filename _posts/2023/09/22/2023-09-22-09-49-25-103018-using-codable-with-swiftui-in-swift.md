---
layout: post
title: "Using Codable with SwiftUI in Swift"
description: " "
date: 2023-09-22
tags: [SwiftUISwiftCodableJSON]
comments: true
share: true
---

With the introduction of SwiftUI in Swift, building user interfaces has become even more intuitive and interactive. When working with network requests and API data, handling JSON responses is a common task. Fortunately, Swift provides a powerful feature called Codable, which allows us to seamlessly convert JSON data into Swift objects and vice versa.

## What is Codable?

Codable is a protocol in Swift that combines the functionalities of `Encodable` and `Decodable`. It allows us to easily encode Swift objects into JSON data and decode JSON data into Swift objects.

## Using Codable with SwiftUI

Using Codable with SwiftUI is straightforward and can greatly simplify working with JSON data in your app.

Let's say we have a JSON response from an API that looks like this:

```swift
{
   "name": "John Doe",
   "age": 30,
   "email": "johndoe@example.com"
}
```

To decode this JSON into a Swift object, we need to create a struct conforming to the `Decodable` protocol. This struct should have properties that match the keys in the JSON:

```swift
struct User: Decodable {
    let name: String
    let age: Int
    let email: String
}
```

To decode the JSON data into a `User` object, we can use the `JSONDecoder` class:

```swift
let jsonString = """
{
   "name": "John Doe",
   "age": 30,
   "email": "johndoe@example.com"
}
"""

let jsonData = jsonString.data(using: .utf8)

do {
    let decoder = JSONDecoder()
    let user = try decoder.decode(User.self, from: jsonData!)
    // Use the user object in your SwiftUI views
} catch {
    print("Error decoding JSON: \(error)")
}
```

Once we have a decoded `User` object, we can use it in our SwiftUI views. For example, we can display the user's name and age:

```swift
struct UserView: View {
    let user: User
    
    var body: some View {
        VStack {
            Text(user.name)
                .font(.title)
            
            Text("Age: \(user.age)")
                .font(.subheadline)
        }
    }
}
```

## Encoding Swift objects to JSON

To encode a Swift object into JSON data, we can use the `JSONEncoder` class. For example, let's say we have a `User` object and want to convert it to JSON:

```swift
let user = User(name: "Jane Doe", age: 25, email: "janedoe@example.com")

do {
    let encoder = JSONEncoder()
    let jsonData = try encoder.encode(user)
    let jsonString = String(data: jsonData, encoding: .utf8)
    // Use the jsonString or send it in a network request
} catch {
    print("Error encoding JSON: \(error)")
}
```

## Conclusion

Using Codable with SwiftUI in Swift makes it incredibly easy to work with JSON data in your app. By conforming your structs to the `Decodable` and `Encodable` protocols, you can seamlessly decode JSON into Swift objects and encode Swift objects to JSON. This simplifies working with API responses and allows you to focus on building beautiful user interfaces with SwiftUI.

#SwiftUISwiftCodableJSON