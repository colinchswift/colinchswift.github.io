---
layout: post
title: "Using JSONSchema with Codable in Swift"
description: " "
date: 2023-09-27
tags: [Swift, JSONSchema]
comments: true
share: true
---

## Introduction

In Swift, the Codable protocol provides a convenient way to encode and decode data into JSON. However, there may be cases where you need to validate incoming JSON data against a specified schema before decoding it. This is where JSONSchema comes into play. In this blog post, we will explore how to use JSONSchema with Codable in Swift.

## Prerequisites

To follow along with the examples in this blog post, you should have basic knowledge of Swift programming language and Codable protocol.

## Installing JSONSchema

Before we proceed, let's install the JSONSchema library using CocoaPods. Open your terminal and navigate to your project folder. Run the following command:

```
$ pod init
```

This will create a Podfile in your project folder. Open the Podfile and add the following line:

```
pod 'JSONSchema', '~> 2.0'
```

Save the Podfile and then run the following command in the terminal:

```
$ pod install
```

Wait for the installation to complete. Once done, close any open project window and open the newly generated ".xcworkspace" file.

## Creating a JSON Schema

First, we need to create a JSON schema for our data model. A JSON schema describes the structure and constraints of the JSON data. Let's say we have a simple User object with two properties: `name` and `age`.

```swift
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "name": {
      "type": "string"
    },
    "age": {
      "type": "number"
    }
  },
  "required": ["name", "age"]
}
```

Save this JSON in a file called "UserSchema.json".

## Connecting JSONSchema with Codable

Let's create a User struct in Swift that will conform to Codable protocol and use JSONSchema for validation.

```swift
import JSONSchema

struct User: Codable {
    let name: String
    let age: Int
}
```

To validate the incoming JSON against the schema, we can create a custom decoder that uses JSONSchema to validate the JSON data.

```swift
import JSONSchema

struct CustomDecoder: JSONDecoderType {
    let schema: JSONSchema

    func decode<T>(_ type: T.Type, from data: Data) throws -> T where T: Decodable {
        guard let json = try JSONSerialization.jsonObject(with: data, options: []) as? [String: Any] else {
            throw DecodingError.dataCorruptedError(in: data, debugDescription: "Invalid JSON")
        }

        let validationResult = try schema.validate(json)
        if !validationResult.valid {
            throw DecodingError.dataCorruptedError(in: data, debugDescription: validationResult.errors.description)
        }

        return try decode(type, from: data)
    }
}
```

Now, let's use the custom decoder in our code to validate and decode the JSON data.

```swift
import JSONSchema

let data = // your JSON data here
let schemaData = // load UserSchema.json

let schema = try JSONSchema(data: schemaData)
let decoder = CustomDecoder(schema: schema)

let user = try decoder.decode(User.self, from: data)
```

This will validate the incoming JSON data against the schema before decoding it into a User object. If the validation fails, the decoding process will halt and throw an error.

## Conclusion

Using JSONSchema with Codable in Swift provides a powerful way to validate JSON data before decoding it into Swift objects. By following the steps outlined in this blog post, you can easily integrate JSONSchema into your Codable workflows and ensure that your data conforms to the specified schema.

#Swift #JSONSchema #Codable