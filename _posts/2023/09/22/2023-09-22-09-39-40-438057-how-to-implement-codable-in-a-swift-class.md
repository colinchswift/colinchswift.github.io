---
layout: post
title: "How to implement Codable in a Swift class"
description: " "
date: 2023-09-22
tags: [codable]
comments: true
share: true
---

Codable is a protocol introduced in Swift 4 to easily encode and decode JSON data. By conforming to Codable, you can easily convert your class instances to and from JSON without writing manual parsing code. In this blog post, we will learn how to implement Codable in a Swift class.

## Step 1: Conforming to Codable

To make a class Codable, you need to conform to the Codable protocol. This can be done by adding ": Codable" to your class declaration. Let's consider an example where we have a Person class with two properties: name and age.

```swift
class Person: Codable {
    var name: String
    var age: Int
    
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
}
```

## Step 2: Encoding to JSON

To convert an instance of your Codable class to JSON, you need to use an instance of JSONEncoder. You can then call the encode method passing in your class instance. Let's see an example:

```swift
let person = Person(name: "John Doe", age: 25)

let encoder = JSONEncoder()
encoder.outputFormatting = .prettyPrinted

do {
    let jsonData = try encoder.encode(person)
    if let jsonString = String(data: jsonData, encoding: .utf8) {
        print(jsonString)
    }
} catch {
    print("Encoding failed: \(error.localizedDescription)")
}
```

In the example above, we create a Person instance and an instance of JSONEncoder. We set the outputFormatting property to .prettyPrinted so that the resulting JSON is nicely formatted. Then, we try to encode the person instance into JSON and convert it to a string for printing.

## Step 3: Decoding from JSON

To convert JSON data back into an instance of your Codable class, you need to use an instance of JSONDecoder. You can then call the decode method passing in the JSON data. Let's see an example:

```swift
let json = """
{
    "name": "Jane Smith",
    "age": 30
}
"""

let jsonData = Data(json.utf8)

let decoder = JSONDecoder()

do {
    let person = try decoder.decode(Person.self, from: jsonData)
    print(person.name)
    print(person.age)
} catch {
    print("Decoding failed: \(error.localizedDescription)")
}
```

In the example above, we create a JSON string and convert it to Data so that we can pass it to the decode method. We then create an instance of JSONDecoder and try to decode the JSON data into a Person instance. Finally, we can access the properties of the decoded person instance.

## Conclusion

By conforming to the Codable protocol, you can easily encode and decode your Swift class instances to and from JSON. This makes it much simpler to work with JSON data in your iOS or macOS apps. Codable provides a clean and efficient way to handle JSON serialization and deserialization. So start implementing Codable in your classes and simplify your JSON data handling!

#swift #codable