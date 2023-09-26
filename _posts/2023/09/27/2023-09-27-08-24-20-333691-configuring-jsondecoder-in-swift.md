---
layout: post
title: "Configuring JSONDecoder in Swift"
description: " "
date: 2023-09-27
tags: [JSONDecoder]
comments: true
share: true
---

When working with JSON data in Swift, there are times when you need to configure the behavior of JSONDecoder to suit your specific requirements. JSONDecoder is a powerful tool provided by the Swift standard library for decoding JSON data into Swift types. In this blog post, we will explore how to configure JSONDecoder to handle different scenarios.

## 1. Changing the key decoding strategy

By default, JSONDecoder assumes that the property names in your Swift types match the keys found in the JSON data. However, there might be cases where the keys in the JSON data are formatted differently or have different naming conventions. For instance, the JSON data may use snake_case while your Swift types use camelCase.

To handle this, you can change the key decoding strategy of JSONDecoder by configuring its `keyDecodingStrategy` property. Here's an example that converts snake_case keys to camelCase:

```swift
struct Person: Codable {
    let firstName: String
    let lastName: String
}

let json = """
{
    "first_name": "John",
    "last_name": "Doe"
}
"""

let decoder = JSONDecoder()
decoder.keyDecodingStrategy = .convertFromSnakeCase

do {
    let person = try decoder.decode(Person.self, from: json.data(using: .utf8)!)
    print(person.firstName) // Output: John
    print(person.lastName) // Output: Doe
} catch {
    print("Error: \(error)")
}
```

In the above example, we set the `keyDecodingStrategy` of the decoder to `.convertFromSnakeCase`. This tells the decoder to automatically convert the JSON keys from snake_case to camelCase and match them with the corresponding properties in the `Person` struct.

## 2. Handling date formats

Another common scenario is when the JSON data contains dates in a specific format, and you need to decode them into Swift `Date` objects. By default, JSONDecoder expects dates in either ISO 8601 or UNIX timestamp format.

To parse custom date formats, you can set the `dateDecodingStrategy` property of JSONDecoder. Here's an example that handles a date string in the format "dd/MM/yyyy":

```swift
struct Article: Codable {
    let title: String
    let publishDate: Date
}

let json = """
{
    "title": "Using JSONDecoder in Swift",
    "publish_date": "15/07/2022"
}
"""

let decoder = JSONDecoder()
let dateFormatter = DateFormatter()
dateFormatter.dateFormat = "dd/MM/yyyy"
decoder.dateDecodingStrategy = .formatted(dateFormatter)

do {
    let article = try decoder.decode(Article.self, from: json.data(using: .utf8)!)
    print(article.title) // Output: Using JSONDecoder in Swift
    print(article.publishDate) // Output: 2022-07-15 00:00:00 +0000
} catch {
    print("Error: \(error)")
}
```

In this example, we set a custom `dateFormatter` with the format "dd/MM/yyyy" and assign it to the `dateDecodingStrategy` property. The decoder will then use this formatter to parse the date string from the JSON data into a `Date` object.

## Conclusion

JSONDecoder is a versatile tool that allows you to configure various aspects of JSON decoding in Swift. In this blog post, we covered how to change the key decoding strategy to handle different naming conventions and how to handle custom date formats. By understanding and leveraging these configuration options, you can make your JSON decoding process more flexible and efficient.

#swift #JSONDecoder