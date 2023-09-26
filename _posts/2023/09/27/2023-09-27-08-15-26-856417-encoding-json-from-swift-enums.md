---
layout: post
title: "Encoding JSON from Swift enums"
description: " "
date: 2023-09-27
tags: [enums, json]
comments: true
share: true
---

When working with APIs, it is common to encode and decode data in JSON format. In Swift, enums are a powerful tool for modeling data with a finite set of possible values. However, when it comes to encoding enums to JSON, there are a few things to consider.

To encode a Swift enum to JSON, you need to convert it into a format that the JSON encoder understands. Here are a few approaches you can take:

1. **Using raw values**: If your enum has raw values defined, you can simply use the raw value for encoding. For example:

```swift
enum Status: String, Codable {
    case active
    case inactive
}

let status = Status.active
let encoder = JSONEncoder()

do {
    let jsonData = try encoder.encode(status.rawValue)
    let jsonString = String(data: jsonData, encoding: .utf8)
    print(jsonString ?? "")
} catch {
    print("Error encoding enum: \(error)")
}
```

In this example, the `Encoder` uses the `rawValue` property of the `Status` enum to encode the enum into a JSON string.

2. **Custom encoding**: If your enum doesn't have raw values or requires custom encoding, you can implement the `Codable` protocol and provide your own encoding logic. For example:

```swift
enum Status: Codable {
    case active(date: Date)
    case inactive

    enum CodingKeys: CodingKey {
        case active
        case inactive
    }

    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)

        switch self {
        case .active(let date):
            try container.encode(date, forKey: .active)
        case .inactive:
            try container.encode(true, forKey: .inactive)
        }
    }
}

let status = Status.active(date: Date())
let encoder = JSONEncoder()

do {
    let jsonData = try encoder.encode(status)
    let jsonString = String(data: jsonData, encoding: .utf8)
    print(jsonString ?? "")
} catch {
    print("Error encoding enum: \(error)")
}
```

In this example, we implement the `encode(to:)` function in the `Codable` protocol to provide custom encoding logic for the `Status` enum.

Remember to handle potential errors during the encoding process using `try-catch` blocks.

By using these approaches, you can easily encode Swift enums to JSON format. This allows you to communicate with APIs and exchange data seamlessly. Happy coding!

#swift #enums #json #encoding