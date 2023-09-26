---
layout: post
title: "Encoding and decoding JSON with schema validation in Swift"
description: " "
date: 2023-09-27
tags: [JSON, schema]
comments: true
share: true
---

JSON (JavaScript Object Notation) is a lightweight data-interchange format that is widely used for representing data structures. In Swift, the `Codable` protocol provides a convenient way to encode and decode JSON. However, it does not offer built-in schema validation to ensure that the decoded JSON matches a specific structure. In this blog post, we will discuss how to encode and decode JSON with schema validation in Swift using a third-party library called `JSONSchema.swift`.

## What is JSONSchema.swift?

`JSONSchema.swift` is a powerful Swift library that provides schema validation capabilities for JSON data. It allows you to define and validate the structure and constraints of JSON objects using the JSON Schema standard.

## Installing JSONSchema.swift

To use `JSONSchema.swift` in your project, you need to add it as a dependency using Swift Package Manager (SPM). Here are the steps to add the library to your project:

1. Open your Xcode project or create a new one.
2. Navigate to **File -> Swift Packages -> Add Package Dependency**.
3. In the package repository URL field, enter the following URL: `https://github.com/kylef/JSONSchema.swift.git`.
4. Choose the desired version or branch.
5. Xcode will resolve the package and add it to your project.

## Encoding JSON

To encode Swift objects to JSON, you can simply conform your types to the `Codable` protocol and use the `JSONEncoder` class. However, if you want to ensure that the encoded JSON matches a specific schema, you can use `JSONSchema.swift` for schema validation. Here's an example:

```swift
import JSONSchema

struct Person: Codable {
    let name: String
    let age: Int
}

let person = Person(name: "John Doe", age: 30)
let encoder = JSONEncoder()

if let jsonData = try? encoder.encode(person),
   let jsonString = String(data: jsonData, encoding: .utf8) {

    // Validate the encoded JSON against a schema
    let schema = try? JSONSchema.fromJSONFile("person_schema.json")
    let validationResult = try? schema?.validate(jsonString)

    if validationResult?.isValid == true {
        print("JSON is valid!")
    } else {
        print("JSON is invalid.")
    }
}
```

In the example above, we define a `Person` struct that conforms to the `Codable` protocol. We then use the `JSONEncoder` class to encode a `person` instance into JSON data. Next, we convert the data to a JSON string and validate it against a schema using `JSONSchema.swift`. If the validation is successful, we print out a success message; otherwise, we print an error message.

## Decoding JSON

Similarly, when decoding JSON with schema validation, you can use the `JSONDecoder` class and `JSONSchema.swift` for validation. Here's an example:

```swift
import JSONSchema

struct Person: Codable {
    let name: String
    let age: Int
}

let jsonString = """
{
    "name": "John Doe",
    "age": 30
}
"""

// Validate the JSON string against a schema
let schema = try? JSONSchema.fromJSONFile("person_schema.json")
let validationResult = try? schema?.validate(jsonString)

if validationResult?.isValid == true {
    let decoder = JSONDecoder()
    if let jsonData = jsonString.data(using: .utf8),
       let person = try? decoder.decode(Person.self, from: jsonData) {
        print(person)
    }
} else {
    print("JSON is invalid.")
}
```

In this example, we have a JSON string representing a `Person` object. We first validate the JSON string against a schema using `JSONSchema.swift`, and if it passes the validation, we can safely decode it into a `Person` object using the `JSONDecoder` class.

## Conclusion

In this blog post, we learned how to encode and decode JSON with schema validation in Swift using the `JSONSchema.swift` library. By combining the `Codable` protocol with schema validation, we can ensure that our JSON data adheres to specific structures and constraints. This helps in maintaining data integrity and increasing the robustness of our applications.

#swift #JSON #schema #validation