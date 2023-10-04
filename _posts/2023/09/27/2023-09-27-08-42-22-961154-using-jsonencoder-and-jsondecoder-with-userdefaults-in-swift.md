---
layout: post
title: "Using JSONEncoder and JSONDecoder with UserDefaults in Swift"
description: " "
date: 2023-09-27
tags: [UserDefaults]
comments: true
share: true
---

In Swift, we often use `UserDefaults` to save and retrieve simple data types like strings, numbers, and Booleans. However, what if we want to store more complex data structures, such as custom objects or arrays? This is where `JSONEncoder` and `JSONDecoder` come in handy.

`JSONEncoder` and `JSONDecoder` are part of the `Foundation` framework and provide a convenient way to encode and decode custom types to and from JSON data. By using them in combination with `UserDefaults`, we can easily store and retrieve complex data structures in a format that can be saved and loaded from disk.

## Storing complex data with `UserDefaults`

Let's say we have a custom struct called `Person` that we want to store in `UserDefaults`. 

```swift
struct Person: Codable {
    var name: String
    var age: Int
}
```

To save an instance of this struct to `UserDefaults`, we need to first encode it into JSON data using `JSONEncoder`, and then save that data using `UserDefaults`.

```swift
let person = Person(name: "John Doe", age: 30)

do {
    let encodedData = try JSONEncoder().encode(person)
    
    UserDefaults.standard.set(encodedData, forKey: "personKey")
} catch {
    // Handle encoding error
}
```

Notice that we're using the `Codable` protocol on the `Person` struct. This allows the compiler to automatically synthesize the necessary encoding and decoding methods for us.

## Retrieving complex data from `UserDefaults`

To retrieve the stored `Person` instance from `UserDefaults`, we can use `UserDefaults.standard.data(forKey:)` to get the encoded data, and then decode it using `JSONDecoder`.

```swift
if let encodedData = UserDefaults.standard.data(forKey: "personKey") {
    do {
        let decodedPerson = try JSONDecoder().decode(Person.self, from: encodedData)
        
        print(decodedPerson) // Person(name: "John Doe", age: 30)
    } catch {
        // Handle decoding error
    }
}
```

Again, notice that we're using the `Codable` protocol here to automatically synthesize the decoding method based on the struct's properties.

## Conclusion

By combining `JSONEncoder` and `JSONDecoder` with `UserDefaults`, we can easily store and retrieve complex data structures in Swift. This allows us to save custom objects and arrays to `UserDefaults` without having to worry about converting them to simple types manually.

Using `UserDefaults` for storing small amounts of data is generally recommended. If your data becomes too large or complex, you might want to consider using other data persistence options such as Core Data or SQLite.

#Swift #UserDefaults #JSONEncoder #JSONDecoder