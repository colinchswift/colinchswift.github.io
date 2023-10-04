---
layout: post
title: "Using JSONEncoder's keyEncodingStrategy in Swift"
description: " "
date: 2023-09-27
tags: [JSONEncoder]
comments: true
share: true
---

In Swift, the `JSONEncoder` class provides a powerful way to encode Swift types into JSON data. One of the handy features it offers is the `keyEncodingStrategy` property, which allows you to specify how the keys of your Swift type should be encoded in the resulting JSON.

By default, `JSONEncoder` uses the `.useDefaultKeys` strategy, which simply uses the property names as-is for encoding. However, there are other strategies available that can be useful in certain scenarios.

## JSONEncoder.KeyEncodingStrategy

The `keyEncodingStrategy` property of `JSONEncoder` accepts a `JSONEncoder.KeyEncodingStrategy` value. Here are some of the most commonly used strategies:

- `.useDefaultKeys`: This is the default strategy that uses property names as-is for encoding.

- `.convertToSnakeCase`: This strategy converts camelCase property names to snake_case. For example, a property named `userName` would be encoded as `user_name`.

- `.convertToPascalCase`: This strategy converts camelCase property names to PascalCase. For example, a property named `userName` would be encoded as `UserName`.

- `.custom`: This strategy allows you to define a custom encoding closure for key names. The closure takes a `CodingKey` instance and returns a `CodingKey` instance representing the encoded key name.

For instance, let's say you have a `User` struct with properties named `firstName` and `lastName`. You want to convert the property names to lowercase for encoding. Here's how you can achieve that:

```swift
struct User: Codable {
    let firstName: String
    let lastName: String
}

let user = User(firstName: "John", lastName: "Doe")

let encoder = JSONEncoder()
encoder.keyEncodingStrategy = .custom { codingKeys in
    let key = codingKeys.last!
    return AnyCodingKey(stringValue: key.stringValue.lowercased())
}

let jsonData = try encoder.encode(user)
let jsonString = String(data: jsonData, encoding: .utf8)
print(jsonString ?? "")
```

The custom encoding closure takes an array of `CodingKey` instances representing the property hierarchy. In the example above, we know that our `User` struct only has the top-level properties, so we can safely assume that the last key in the array represents the current property we want to encode. We create a custom `CodingKey` instance with the lowercase version of the key's `stringValue` and return it.

The output of the above code would be:

```json
{"firstname":"John","lastname":"Doe"}
```

## Conclusion

The `JSONEncoder` class in Swift provides flexible and customizable options for encoding Swift types into JSON data. The `keyEncodingStrategy` property is a powerful feature that allows you to control how the keys are encoded in the resulting JSON. By using the appropriate key encoding strategy, you can ensure that your JSON data adheres to a specific convention or requirement.

#Swift #JSONEncoder