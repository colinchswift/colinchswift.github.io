---
layout: post
title: "Renaming JSON properties during parsing in Swift"
description: " "
date: 2023-10-24
tags: [Tags, JSON]
comments: true
share: true
---

When working with JSON data in Swift, it's common to encounter situations where you want to rename some of the properties during the parsing process. This can be useful when the JSON keys don't match your preferred naming convention or when you need to handle versioning changes in your API.

In this article, we will explore how to easily rename JSON properties during parsing in Swift by leveraging the Codable protocol and custom key strategies.

## Table of Contents

- [Introduction](#introduction)
- [Using Custom CodingKeys](#using-custom-codingkeys)
- [Handling API Changes](#handling-api-changes)
- [Conclusion](#conclusion)

## Introduction

The Codable protocol introduced in Swift 4 provides a convenient way to encode and decode JSON data. By conforming your model objects to the Codable protocol, you can easily convert them to and from JSON representations.

To rename JSON properties during parsing, we need to override the default behavior of the Codable system by creating a custom implementation of the `CodingKey` protocol.

## Using Custom CodingKeys

The `CodingKey` protocol allows us to map between the JSON keys and our desired property names.

Consider the following JSON data:

```json
{
  "first_name": "John",
  "last_name": "Doe"
}
```

Let's say we want to parse this JSON into a Swift object with properties `firstName` and `lastName`, instead of `first_name` and `last_name`. We can achieve this by defining a custom `CodingKeys` enum inside our model struct or class:

```swift
struct Person: Codable {
  let firstName: String
  let lastName: String

  private enum CodingKeys: String, CodingKey {
    case firstName = "first_name"
    case lastName = "last_name"
  }
}
```
Here, we map the JSON key `"first_name"` to the Swift property `firstName` using the `CodingKeys` enum. The same applies to `"last_name"` and `lastName`.

Now, when we decode the JSON, the `firstName` and `lastName` properties will be properly renamed.

```swift
let jsonData = """
{
  "first_name": "John",
  "last_name": "Doe"
}
""".data(using: .utf8)!

let decoder = JSONDecoder()
let person = try decoder.decode(Person.self, from: jsonData)

print(person.firstName) // Output: John
print(person.lastName) // Output: Doe
```

## Handling API Changes

One of the main benefits of renaming JSON properties during parsing is the ability to handle API changes gracefully.

Consider a scenario where the API has changed, and the JSON key `"first_name"` has been renamed to `"firstName"`:

```json
{
  "firstName": "John",
  "last_name": "Doe"
}
```

To handle this change, all we need to do is update our `CodingKeys` enum:

```swift
struct Person: Codable {
  let firstName: String
  let lastName: String

  private enum CodingKeys: String, CodingKey {
    case firstName = "first_name" // Changed from "first_name"
    case lastName = "last_name"
  }
}
```

By renaming the corresponding enum case from `"first_name"` to `"firstName"`, we can seamlessly adapt to the new API without needing to update the rest of our code.

## Conclusion

In this article, we explored how to rename JSON properties during parsing in Swift by using the Codable protocol and custom `CodingKeys`. By mapping the JSON keys to our preferred property names, we can easily handle naming conventions and changes in the API.

This technique allows us to have cleaner and more readable code, enabling better maintenance and adaptability to future API changes.

## References

- [Swift Codable Documentation](https://developer.apple.com/documentation/swift/codable) 

#Tags: Swift #JSON #Parsing