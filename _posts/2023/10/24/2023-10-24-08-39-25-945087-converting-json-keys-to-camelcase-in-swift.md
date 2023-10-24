---
layout: post
title: "Converting JSON keys to CamelCase in Swift"
description: " "
date: 2023-10-24
tags: []
comments: true
share: true
---

When working with JSON in Swift, it is common to receive data with keys in snake_case format. However, the preferred convention in Swift is to use camelCase for variable and property names. Therefore, it is necessary to convert the snake_case keys to CamelCase in order to match the Swift naming convention.

In this article, we will explore how to convert JSON keys to CamelCase in Swift using a few different approaches.

## 1. Manual Conversion

The simplest way to convert snake_case keys to CamelCase in Swift is by manually iterating over the JSON and converting the keys. Here's how you can do it:

```swift
func convertJSONKeysToCamelCase(json: [String: Any]) -> [String: Any] {
    var convertedJSON: [String: Any] = [:]
    
    for (key, value) in json {
        let convertedKey = key.camelCased() // Convert key to CamelCase
        
        if let innerJSON = value as? [String: Any] {
            convertedJSON[convertedKey] = convertJSONKeysToCamelCase(json: innerJSON)
        } else if let innerArray = value as? [[String: Any]] {
            let convertedArray = innerArray.map { convertJSONKeysToCamelCase(json: $0) }
            convertedJSON[convertedKey] = convertedArray
        } else {
            convertedJSON[convertedKey] = value
        }
    }
    
    return convertedJSON
}

extension String {
    func camelCased() -> String {
        let words = self.components(separatedBy: "_")
        let camelCasedWords = words.map { $0.capitalized }
        return camelCasedWords.joined()
    }
}
```

In the `convertJSONKeysToCamelCase` function, we iterate over the JSON key-value pairs and convert the keys to CamelCase using the `camelCased()` function defined in a string extension. If the value is another dictionary or an array of dictionaries, we recursively call the `convertJSONKeysToCamelCase` function to convert the nested keys as well.

## 2. Codable Protocol

Another approach to convert JSON keys to CamelCase in Swift is by utilizing the `Codable` protocol. By customizing the `CodingKeys` enumeration, we can specify the mapping between the snake_case keys in the JSON and the camelCase property names in the Swift struct or class.

Here's an example:

```swift
struct Person: Codable {
    let firstName: String
    let lastName: String
    let age: Int
    
    private enum CodingKeys: String, CodingKey {
        case firstName = "first_name"
        case lastName = "last_name"
        case age
    }
}

let json = """
{
    "first_name": "John",
    "last_name": "Doe",
    "age": 30
}
"""

let data = json.data(using: .utf8)!
let person = try JSONDecoder().decode(Person.self, from: data)
print(person.firstName) // Output: "John"
print(person.lastName) // Output: "Doe"
print(person.age) // Output: 30
```

In this example, the `CodingKeys` enumeration defines the mapping between the snake_case keys in the JSON and the camelCase property names in the `Person` struct. By conforming to the `Codable` protocol and customizing the `CodingKeys` enumeration, the JSONDecoder automatically converts the snake_case keys to camelCase property names during decoding.

## Conclusion

Converting JSON keys from snake_case to CamelCase is a common task when working with JSON data in Swift. In this article, we explored two different approaches to achieve this conversion. The first approach is a manual conversion using a recursive function, while the second approach utilizes the `Codable` protocol and custom `CodingKeys` enumeration. 

Both approaches have their own advantages and can be used depending on your specific requirements and preferences.