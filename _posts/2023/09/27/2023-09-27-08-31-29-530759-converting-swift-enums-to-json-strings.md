---
layout: post
title: "Converting Swift enums to JSON strings"
description: " "
date: 2023-09-27
tags: [coding, enums]
comments: true
share: true
---
title: Converting Swift Enums to JSON Strings
date: 2021-09-15
description: Learn how to convert Swift enums to JSON strings using Codable protocol.
tags: swift, coding, enums, json
---

# Converting Swift Enums to JSON Strings

One common task in Swift development is converting data structures to different formats, such as JSON. When working with enums, it's important to know how to convert them to JSON strings. In this blog post, we will explore how to convert Swift enums to JSON strings using the Codable protocol.

## Understanding the Codable Protocol

The Codable protocol in Swift provides a convenient way to encode and decode data. It can be used with many different data types, including enums. By adopting the Codable protocol, we can easily convert our enums to JSON strings.

## Converting Enums to JSON Strings

First, let's define an enum that we want to convert to a JSON string:

```swift
enum Priority: String, Codable {
    case low
    case medium
    case high
}
```

To convert this enum to a JSON string, we need to create an instance of JSONEncoder and call its encode method:

```swift
let priority = Priority.high
let encoder = JSONEncoder()
let jsonData = try encoder.encode(priority)
let jsonString = String(data: jsonData, encoding: .utf8)
```

In the example above, we define a priority enum with three cases: low, medium, and high. We then create an instance of JSONEncoder and use it to encode the priority enum. The resulting JSON data is converted to a string using the String initializer.

## Converting JSON Strings to Enums

To convert JSON strings back to enums, we need to define the Decodable protocol for our enum:

```swift
enum Priority: String, Codable {
    case low
    case medium
    case high
}
```

Using the JSONDecoder, we can convert the JSON string back to the enum:

```swift
let jsonString = """
    "high"
"""
let jsonData = jsonString.data(using: .utf8)!
let decoder = JSONDecoder()
let priority = try decoder.decode(Priority.self, from: jsonData)
```

In the example above, we define a JSON string with the value "high". We then convert this string to JSON data and use the JSONDecoder to decode it back into a priority enum.

## Conclusion

Using the Codable protocol, we can easily convert Swift enums to JSON strings. By adopting both the Codable and Decodable protocols, we can convert both ways between enums and JSON strings. This allows us to seamlessly work with enums in JSON-based APIs or data stores.

Remember to import the Foundation module when working with JSON encoding and decoding in Swift.

#swift #coding #enums #json