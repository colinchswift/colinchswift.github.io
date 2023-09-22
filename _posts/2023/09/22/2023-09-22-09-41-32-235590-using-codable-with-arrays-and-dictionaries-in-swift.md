---
layout: post
title: "Using Codable with arrays and dictionaries in Swift"
description: " "
date: 2023-09-22
tags: [swift, Codable]
comments: true
share: true
---

Swift's Codable protocol provides an elegant way to encode and decode data into various formats, such as JSON and property lists. While Codable is commonly used to encode and decode individual objects, it can also be used with arrays and dictionaries to serialize and deserialize collections of data. In this blog post, we will explore how to use Codable with arrays and dictionaries in Swift.

## Encoding Arrays and Dictionaries

Encoding an array or dictionary using Codable is a straightforward process. Let's assume we have a struct called `Person` with two properties: `name` of type `String` and `age` of type `Int`.

```swift
struct Person: Codable {
    let name: String
    let age: Int
}
```

To encode an array of `Person` objects, we simply encode the array itself using an instance of `JSONEncoder`.

```swift
let people = [Person(name: "John", age: 25),
              Person(name: "Jane", age: 30)]

let encoder = JSONEncoder()
if let encodedData = try? encoder.encode(people),
   let jsonString = String(data: encodedData, encoding: .utf8) {
    print(jsonString)
} else {
    print("Encoding failed.")
}
```

The above code creates an array of `Person` objects and encodes it into JSON format using `JSONEncoder`. The resulting JSON string is then printed to the console.

Similarly, encoding a dictionary follows the same process. Let's assume we have a dictionary where the keys are `String` and the values are `Person` objects.

```swift
let peopleDictionary = ["John": Person(name: "John", age: 25),
                        "Jane": Person(name: "Jane", age: 30)]

let encoder = JSONEncoder()
if let encodedData = try? encoder.encode(peopleDictionary),
   let jsonString = String(data: encodedData, encoding: .utf8) {
    print(jsonString)
} else {
    print("Encoding failed.")
}
```

In this case, the dictionary is encoded using `JSONEncoder` and the resulting JSON string is printed.

## Decoding Arrays and Dictionaries

Decoding arrays and dictionaries is equally effortless. We can take the same `Person` struct and decode an array or dictionary of `Person` objects from a JSON string.

```swift
let jsonString = """
                 [{"name": "John", "age": 25},
                  {"name": "Jane", "age": 30}]
                 """

let decoder = JSONDecoder()
if let jsonData = jsonString.data(using: .utf8),
   let people = try? decoder.decode([Person].self, from: jsonData) {
    for person in people {
        print(person.name, person.age)
    }
} else {
    print("Decoding failed.")
}
```

In the above code, we have a JSON string representing an array of `Person` objects. The JSON string is then decoded into an array using `JSONDecoder`. We can iterate over the decoded array and access each `Person` object's properties.

Decoding a dictionary follows a similar approach. Let's assume we have a JSON string representing a dictionary where the keys are `String` and the values are `Person` objects.

```swift
let jsonString = """
                 {
                     "John": {"name": "John", "age": 25},
                     "Jane": {"name": "Jane", "age": 30}
                 }
                 """

let decoder = JSONDecoder()
if let jsonData = jsonString.data(using: .utf8),
   let peopleDictionary = try? decoder.decode([String: Person].self, from: jsonData) {
    for (key, value) in peopleDictionary {
        print(key, value.name, value.age)
    }
} else {
    print("Decoding failed.")
}
```

Here, the JSON string representing a dictionary is decoded into a `[String: Person]` dictionary using `JSONDecoder`. We can iterate over the decoded dictionary and access each `Person` object's properties.

## Conclusion

Swift's Codable protocol makes it easy to encode and decode arrays and dictionaries. By conforming to `Codable` and using `JSONEncoder` and `JSONDecoder`, we can seamlessly serialize and deserialize collections of data. This feature proves to be invaluable when working with complex data structures in Swift.

#swift #Codable