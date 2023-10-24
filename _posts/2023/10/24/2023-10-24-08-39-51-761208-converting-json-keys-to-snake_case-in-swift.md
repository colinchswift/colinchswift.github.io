---
layout: post
title: "Converting JSON keys to snake_case in Swift"
description: " "
date: 2023-10-24
tags: [json, naming]
comments: true
share: true
---

When working with JSON data in Swift, it is common to need to convert the keys from their original format to `snake_case` for better adherence to coding conventions. In this article, we will explore a simple and efficient approach to convert JSON keys to `snake_case` in Swift.

## Understanding `snake_case` format

`snake_case` is a naming convention where words are separated by underscores ('_') and each word is in lower case. Swift, on the other hand, follows the `camelCase` naming convention for variables and functions.

Converting JSON keys from their original format (which is typically `camelCase` or `PascalCase`) to `snake_case` can enhance code readability and consistency within a project.

## Approach to converting JSON keys

Here is a step-by-step approach to converting JSON keys to `snake_case` in Swift:

1. Deserialize the JSON data into a Swift dictionary using `JSONSerialization` or a JSON decoding library like `Codable`.
   
    ```
    let json = """
    {
        "firstName": "John",
        "lastName": "Doe",
        "emailAddress": "john.doe@example.com"
    }
    """
   
    guard let jsonData = json.data(using: .utf8),
          let jsonObject = try? JSONSerialization.jsonObject(with: jsonData, options: []),
          let dictionary = jsonObject as? [String: Any] else {
        // handle JSON parsing error
        return
    }
    ```

2. Create a function to convert a given string from `camelCase` to `snake_case`. This function will iterate through each character in the string and identify uppercase letters. When an uppercase letter is found, it adds an underscore ('_') before converting the uppercase letter to lowercase.
  
    ```swift
    func convertToSnakeCase(_ input: String) -> String {
        var output = ""
        
        for character in input {
            if character.isUppercase {
                output.append("_")
            }
            
            output.append(character.lowercased())
        }
        
        return output
    }
    ```

    Example usage:
   
    ```swift
    let camelCaseKey = "firstName"
    let snakeCaseKey = convertToSnakeCase(camelCaseKey)
    // snakeCaseKey -> "first_name"
    ```

3. Transform the dictionary keys to `snake_case` using the `mapValues` method and the `convertToSnakeCase` function defined above.
  
    ```swift
    let snakeCaseDictionary = dictionary.mapValues { value in
        if let nestedDictionary = value as? [String: Any] {
            return convertKeysToSnakeCase(nestedDictionary)
        } else {
            return value
        }
    }
    ```

    The above code snippet handles nested dictionaries as well, recursively converting the keys to `snake_case`.

4. Serialize the transformed dictionary back into JSON format if needed.
  
    ```swift
    if let jsonData = try? JSONSerialization.data(withJSONObject: snakeCaseDictionary, options: .prettyPrinted) {
        let jsonString = String(data: jsonData, encoding: .utf8)
        print(jsonString)
    }
    ```

With the above approach, you can easily convert JSON keys from `camelCase` or any other format to `snake_case` in Swift. This will make your code more consistent and readable, improving collaboration and maintainability.

## Conclusion

Converting JSON keys to `snake_case` is a common task when working with APIs and JSON data in Swift. By following the steps outlined in this article, you can easily convert the keys to the desired naming convention. Remember to use consistent naming conventions throughout your codebase for clarity and best coding practices.

### References
- [JSONSerialization - Apple Developer Documentation](https://developer.apple.com/documentation/foundation/jsonserialization)
- [Codable - Apple Developer Documentation](https://developer.apple.com/documentation/swift/codable) 

#swift #json #naming-convention