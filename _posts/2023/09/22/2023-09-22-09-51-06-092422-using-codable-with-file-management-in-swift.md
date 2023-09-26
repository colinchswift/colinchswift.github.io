---
layout: post
title: "Using Codable with file management in Swift"
description: " "
date: 2023-09-22
tags: [Codable]
comments: true
share: true
---

In Swift, the Codable protocol provides a convenient way to encode and decode Swift types to and from various formats such as JSON, plist, and even binary. This makes it incredibly useful for working with file management in Swift. In this blog post, we will explore how to use Codable with file management to easily save and retrieve Swift objects.

## Saving Codable Objects to a File

To save a Codable object to a file, we can use the FileManager class to write the encoded data to a file on disk. Let's assume we have a Codable struct named `Person` with name and age properties:

```swift
struct Person: Codable {
    let name: String
    let age: Int
}
```

We can create an instance of `Person` and encode it into JSON format, and then save it to a file:

```swift
let person = Person(name: "John Doe", age: 25)

let jsonEncoder = JSONEncoder()
if let data = try? jsonEncoder.encode(person) {
    let documentDirectory = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask).first
    let fileURL = documentDirectory?.appendingPathComponent("person.json")
    try? data.write(to: fileURL)
}
```

The above code first encodes the `person` object into JSON format using a JSONEncoder. Then, it retrieves the document directory URL using FileManager, appends the desired file name to that URL, and writes the encoded data to that file.

## Retrieving Codable Objects from a File

To retrieve a Codable object from a file, we can use FileManager to read the contents of the file, and then decode the data into the desired Swift type. Let's assume we want to retrieve the `person` object we saved earlier:

```swift
let documentDirectory = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask).first
let fileURL = documentDirectory?.appendingPathComponent("person.json")

if let data = try? Data(contentsOf: fileURL) {
    let jsonDecoder = JSONDecoder()
    if let person = try? jsonDecoder.decode(Person.self, from: data) {
        print(person)
    }
}
```

In this code, we first retrieve the URL of the file we want to read. Then, we use Data(contentsOf:) to read the contents of the file into data. We then create a JSONDecoder and attempt to decode the data into an instance of `Person`. If successful, we can access and use our retrieved `person` object.

## Conclusion

Using Codable in conjunction with file management in Swift provides a simple and efficient way to save and retrieve Swift objects. By leveraging the power of encoding and decoding, we can easily work with different file formats and persist data on disk. This makes working with files in a Swift application much more streamlined and convenient.

#Swift #Codable #FileManagement