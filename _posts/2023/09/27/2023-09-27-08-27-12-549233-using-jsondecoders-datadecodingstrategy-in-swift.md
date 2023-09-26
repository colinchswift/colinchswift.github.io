---
layout: post
title: "Using JSONDecoder's dataDecodingStrategy in Swift"
description: " "
date: 2023-09-27
tags: []
comments: true
share: true
---

In Swift, `JSONDecoder` is a powerful class that allows us to easily convert JSON data into Swift objects. By default, it assumes that the JSON data is represented as UTF-8 encoded data. However, there might be cases where the JSON data is encoded using a different format, such as Base64 or custom encoding.

To handle these situations, we can use the `dataDecodingStrategy` property of `JSONDecoder`. `dataDecodingStrategy` allows us to specify a custom strategy for decoding the data.

## Example

Let's say we have a JSON payload that contains a `data` field, which is Base64 encoded. We want to decode this JSON data into a Swift struct. Here's an example of how we can accomplish this using `dataDecodingStrategy`:

```swift
struct MyStruct: Codable {
    let value: Data
}

let jsonString = """
{
    "value": "SGVsbG8gd29ybGQhCg=="
}
"""

let decoder = JSONDecoder()
decoder.dataDecodingStrategy = .base64 // Set the data decoding strategy to Base64

let jsonData = jsonString.data(using: .utf8)!
let myStruct = try decoder.decode(MyStruct.self, from: jsonData)

print(String(data: myStruct.value, encoding: .utf8)) // Output: "Hello world!"
```

In the above example, we define a `MyStruct` struct with a `value` property of type `Data`. We set the `dataDecodingStrategy` of the `JSONDecoder` instance to `.base64`, indicating that the `value` field should be decoded as Base64 data. When we decode the JSON data using `decode(_:from:)`, the `value` field will be automatically decoded from Base64 to a `Data` object.

## Conclusion

`JSONDecoder`'s `dataDecodingStrategy` property provides us with flexibility when decoding JSON data in different formats. By using the appropriate decoding strategy, we can handle cases where the JSON data is encoded using a format other than UTF-8.