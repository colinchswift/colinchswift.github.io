---
layout: post
title: "Using ObjectMapper library for JSON parsing in Swift"
description: " "
date: 2023-10-24
tags: []
comments: true
share: true
---

Parsing and mapping JSON data is a common task in many iOS applications. In Swift, we can utilize the `ObjectMapper` library to simplify the process of parsing JSON and mapping it to custom model objects.

## What is ObjectMapper?

`ObjectMapper` is a powerful and widely used library in Swift that makes it easy to convert JSON data into Swift objects and vice versa. It provides a simple and declarative way to map JSON to model objects without writing manual parsing code.

## Installation

To start using `ObjectMapper` in your project, you need to include it as a dependency in your `Podfile`:

```swift
pod 'ObjectMapper', '~> 4.2'
```

Then, run `pod install` in your terminal to install the library.

## Mapping JSON to Model Objects

Consider the following JSON response:

```json
{
  "name": "John Doe",
  "age": 25,
  "email": "johndoe@example.com"
}
```

To map this JSON to a Swift object, we need to create a corresponding model object and define its mapping using `ObjectMapper`.

```swift
import ObjectMapper

class User: Mappable {
    var name: String?
    var age: Int?
    var email: String?

    required init?(map: Map) {}

    func mapping(map: Map) {
        name  <- map["name"]
        age   <- map["age"]
        email <- map["email"]
    }
}
```

In the above code, we define a model class `User` that conforms to the `Mappable` protocol provided by `ObjectMapper`. We declare the properties `name`, `age`, and `email` that correspond to the JSON keys. Then, we implement the `mapping(map:)` function where we define the mapping of properties to their respective JSON keys.

To actually parse the JSON data and create a `User` object, we can use the `ObjectMapper`'s `Map` class.

```swift
let jsonString = """
    {
        "name": "John Doe",
        "age": 25,
        "email": "johndoe@example.com"
    }
"""

if let user = Mapper<User>().map(JSONString: jsonString) {
    print(user.name)   // Output: John Doe
    print(user.age)    // Output: 25
    print(user.email)  // Output: johndoe@example.com
}
```

In the above code, we use the `createMap()` function to create a `Map` object and call the `map(JSONString:)` function to parse the JSON string and create a `User` object. We can then access the properties of the `User` object as shown in the print statements.

## Mapping Arrays of Objects

`ObjectMapper` also supports mapping arrays of objects from JSON. Consider the following JSON response:

```json
{
  "users": [
    {
      "name": "John Doe",
      "age": 25,
      "email": "johndoe@example.com"
    },
    {
      "name": "Jane Smith",
      "age": 30,
      "email": "janesmith@example.com"
    }
  ]
}
```

To map this JSON to an array of `User` objects, we can make use of `ObjectMapper`'s `TransformOf` class.

```swift
import ObjectMapper

class UserResponse: Mappable {
    var users: [User]?

    required init?(map: Map) {}

    func mapping(map: Map) {
        users <- (map["users"], ArrayTransform<User>())
    }
}

class ArrayTransform<T: Mappable>: TransformType {
    typealias Object = [T]
    typealias JSON = [Any]

    init() {}

    func transformFromJSON(_ value: Any?) -> [T]? {
        return Mapper<T>().mapArray(JSONObject: value)
    }

    func transformToJSON(_ value: [T]?) -> [Any]? {
        return Mapper<T>().toJSONArray(value)
    }
}
```

In the above code, we define a `UserResponse` class that contains an array of `User` objects. Using the `ArrayTransform` class, we define the transformation for mapping the `users` property from JSON to an array of `User` objects.

To actually parse the JSON data and create the `UserResponse` object with an array of `User` objects, we can use the same `Mapper` class.

```swift
let jsonString = """
    {
        "users": [
            {
                "name": "John Doe",
                "age": 25,
                "email": "johndoe@example.com"
            },
            {
                "name": "Jane Smith",
                "age": 30,
                "email": "janesmith@example.com"
            }
        ]
    }
"""

if let userResponse = Mapper<UserResponse>().map(JSONString: jsonString) {
    if let users = userResponse.users {
        for user in users {
            print(user.name)   // Output: John Doe, Jane Smith
            print(user.age)    // Output: 25, 30
            print(user.email)  // Output: johndoe@example.com, janesmith@example.com
        }
    }
}
```

In the above code, we parse the JSON data using the `Mapper` class and access the array of `User` objects from the `UserResponse` object. We can then iterate over the array and access the properties of each `User` object.

## Conclusion

Using the `ObjectMapper` library in Swift makes JSON parsing and mapping to model objects a breeze. It provides a simple and expressive way to handle JSON data in your iOS applications, saving you time and reducing boilerplate code.

By leveraging `ObjectMapper`, you can easily integrate REST APIs and handle JSON responses efficiently, making your app more robust and maintainable.

**References:**
- [ObjectMapper GitHub Repository](https://github.com/tristanhimmelman/ObjectMapper)