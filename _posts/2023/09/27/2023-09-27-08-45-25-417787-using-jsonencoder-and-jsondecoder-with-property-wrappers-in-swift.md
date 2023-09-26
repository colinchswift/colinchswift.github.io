---
layout: post
title: "Using JSONEncoder and JSONDecoder with property wrappers in Swift"
description: " "
date: 2023-09-27
tags: []
comments: true
share: true
---

In Swift, `JSONEncoder` and `JSONDecoder` are powerful tools for converting between objects and JSON data. They allow you to easily encode Swift objects into JSON and decode JSON data into Swift objects. With the introduction of property wrappers in Swift 5.1, working with `JSONEncoder` and `JSONDecoder` has become even more convenient and expressive.

Let's see an example of how to use property wrappers with `JSONEncoder` and `JSONDecoder`.

## Step 1: Define your model struct

First, let's define a simple `User` struct with some properties that we want to encode and decode from JSON:

```swift
struct User: Codable {
    @JSONCodingKey("user_id")
    var userId: String

    @JSONCodingKey("full_name")
    var fullName: String

    @JSONCodingKey("email_address")
    var emailAddress: String
}
```

In the above code, we have used a property wrapper called `JSONCodingKey` to specify custom JSON keys for our properties. This property wrapper will be responsible for mapping the property names to the corresponding JSON keys during encoding and decoding.

## Step 2: Define the property wrapper

Now, let's define the `JSONCodingKey` property wrapper:

```swift
@propertyWrapper
struct JSONCodingKey {
    let key: String

    init(_ key: String) {
        self.key = key
    }

    var wrappedValue: String {
        get { key }
        set { }
    }

    var projectedValue: String {
        get { key }
        set { }
    }
}
```

In the above code, we define a property wrapper called `JSONCodingKey` that takes a key string as a parameter. The wrapper has two computed properties, `wrappedValue` and `projectedValue`, which return the key value.

## Step 3: Encoding and decoding

Now that we have our model struct and the property wrapper, we can use `JSONEncoder` and `JSONDecoder` to encode and decode our `User` objects.

### Encoding

```swift
let user = User(userId: "12345", fullName: "John Doe", emailAddress: "john.doe@example.com")

let encoder = JSONEncoder()
let encodedData = try encoder.encode(user)

let jsonString = String(data: encodedData, encoding: .utf8)
print(jsonString ?? "")
```

In the above code, we create an instance of `User` and encode it using `JSONEncoder`. The encoded data is then converted to a string representation using `String(data:encoding:)` and printed.

The output will be:

```json
{
    "user_id": "12345",
    "full_name": "John Doe",
    "email_address": "john.doe@example.com"
}
```

### Decoding

```swift
let jsonString = """
{
    "user_id": "12345",
    "full_name": "John Doe",
    "email_address": "john.doe@example.com"
}
"""

let decoder = JSONDecoder()
let decodedUser = try decoder.decode(User.self, from: jsonString.data(using: .utf8)!)

print(decodedUser.userId)
print(decodedUser.fullName)
print(decodedUser.emailAddress)
```

In the above code, we create a JSON string representing a `User` object. We then use `JSONDecoder` to decode the JSON string into a `User` instance. Finally, we print the individual properties of the decoded user.

The output will be:

```
12345
John Doe
john.doe@example.com
```

## Conclusion

Using property wrappers with `JSONEncoder` and `JSONDecoder` in Swift provides a clean and concise way to customize the encoding and decoding process. It allows you to easily map your Swift properties to custom JSON keys without cluttering your model structs with additional coding keys.