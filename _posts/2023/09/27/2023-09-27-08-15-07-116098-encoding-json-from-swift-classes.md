---
layout: post
title: "Encoding JSON from Swift classes"
description: " "
date: 2023-09-27
tags: [JSON]
comments: true
share: true
---

In Swift, you may often need to encode any custom types or classes into JSON format for network communication or data persistence. Thankfully, Swift provides a built-in `JSONEncoder` class that makes this process simple and straightforward. In this blog post, we'll explore how to encode your Swift classes into JSON using `JSONEncoder`.

To begin with, let's consider a simple example where we have a `Person` class with a few properties:

```swift
class Person: Codable {
    var name: String
    var age: Int
    var address: String?

    init(name: String, age: Int, address: String?) {
        self.name = name
        self.age = age
        self.address = address
    }
}
```

To encode an instance of this `Person` class into a JSON object, we will follow these steps:

1. Create an instance of `Person`:

    ```swift
    let person = Person(name: "John Doe", age: 30, address: "123 Main St")
    ```

2. Create an instance of `JSONEncoder`:

    ```swift
    let encoder = JSONEncoder()
    ```

3. Encode the `person` instance into JSON data:

    ```swift
    if let jsonData = try? encoder.encode(person) {
        // Process the encoded JSON data
    }
    ```

In the code snippet above, we use the `encode(_:)` function of `JSONEncoder` to convert the `person` instance into JSON data. The resulting `jsonData` can then be sent over the network, stored in a file, or used in any other way required.

You can also customize the encoding process by setting various properties on the `encoder` instance. For example, you can set the output formatting to pretty-print the JSON:

```swift
encoder.outputFormatting = .prettyPrinted
```

This will add indentation and line breaks to the output JSON.

Additionally, if your Swift class contains properties that should not be encoded, you can use the `CodingKeys` enumeration to specify the coding keys to be used. For example:

```swift
class Person: Codable {
    var name: String
    var age: Int
    var address: String?
    var userType: UserType

    init(name: String, age: Int, address: String?, userType: UserType) {
        self.name = name
        self.age = age
        self.address = address
        self.userType = userType
    }

    private enum CodingKeys: String, CodingKey {
        case name
        case age
        case address
    }
}
```

In the above code, the `userType` property will not be encoded into JSON as it is not included in the `CodingKeys` enumeration.

In conclusion, encoding Swift classes into JSON using `JSONEncoder` is a straightforward process. By following the steps outlined above, you can easily convert your custom types or classes into JSON format for various purposes.

#Swift #JSON-Encoding