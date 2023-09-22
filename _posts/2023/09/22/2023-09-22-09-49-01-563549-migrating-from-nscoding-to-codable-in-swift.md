---
layout: post
title: "Migrating from NSCoding to Codable in Swift"
description: " "
date: 2023-09-22
tags: [swift, codable]
comments: true
share: true
---

In Swift, the `NSCoding` protocol has long been used to archive and unarchive objects, allowing them to be saved to disk or sent over a network. With the introduction of the `Codable` protocol in Swift 4, there is now a more modern and concise way to achieve the same functionality.

## What is NSCoding?

`NSCoding` is a protocol provided by Foundation framework in Swift, used to encode and decode objects. It requires you to implement two methods: `encode(with:)` to encode the object's properties, and `init?(coder:)` to decode the object's properties. While this approach has been widely used, it can be a bit verbose and tedious, especially when dealing with complex object graphs.

## Introduction to Codable

`Codable` is a protocol introduced in Swift 4 that combines the functionalities of `Encodable` and `Decodable`. Adopting `Codable` makes your type **automatically encodable and decodable**.

The `Codable` protocol uses Swift's type inference to encode and decode properties based on their types. It supports all the basic types, as well as custom types, as long as they also conform to the `Codable` protocol.

## Migrating from NSCoding to Codable

So, how do we migrate from `NSCoding` to `Codable`? The good news is that the process is quite straightforward.

1. Conform your class or struct to the `Codable` protocol:
   
   ```swift
   struct MyClass: Codable {
       // properties to be encoded and decoded
   }
   ```

2. Remove the `NSCoding` conformance and its associated methods (`encode(with:)` and `init?(coder:)`) from your class or struct.

3. Encode an object:

   ```swift
   let object = MyClass()
   let encodedData = try JSONEncoder().encode(object)
   ```

4. Decode an object:

   ```swift
   let decodedObject = try JSONDecoder().decode(MyClass.self, from: data)
   ```

## Conclusion

Migrating from `NSCoding` to `Codable` in Swift is a simple and effective process. By adopting the `Codable` protocol, you can take advantage of Swift's powerful type inference and significantly reduce the amount of boilerplate code needed for encoding and decoding objects.

With `Codable`, you can encode and decode objects in a concise and expressive way, making your code more readable and maintainable. So, start migrating your codebase to `Codable` and enjoy the benefits of this modern approach.

#swift #codable