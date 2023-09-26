---
layout: post
title: "Building flexible and extensible data models using Codable in Swift"
description: " "
date: 2023-09-22
tags: [Codable]
comments: true
share: true
---

One of the key features of the Swift programming language is its ability to easily work with data. The Codable protocol introduced in Swift 4 is a powerful tool that allows you to serialize and deserialize data in a straightforward manner. In this blog post, we will explore how to use Codable to build flexible and extensible data models in Swift.

## What is Codable?

Codable is a protocol in Swift that combines the functionality of the Encodable and Decodable protocols. It provides a simple way to encode and decode custom types to and from various formats such as JSON, property lists, and more.

## Declaring Codable Data Models

To make a data model Codable, you need to conform to the Codable protocol. Let's start by creating a simple Person struct as an example:

```swift
struct Person: Codable {
    var name: String
    var age: Int
    var address: String
}
```

By adding the Codable conformance to the Person struct, we are able to encode instances of Person to a JSON representation and decode JSON back into Person instances effortlessly.

## Nested Data Models

Codable also supports encoding and decoding of nested data models. Let's extend the Person struct to include a nested struct called Job:

```swift
struct Person: Codable {
    var name: String
    var age: Int
    var address: String
    var job: Job
}

struct Job: Codable {
    var title: String
    var company: String
    var salary: Double
}
```

With this change, the Person struct now includes the Job struct as a property. You can nest data models as deeply as needed to represent your data structure.

## Customizing Encoding and Decoding

By default, Codable automatically derives the coding keys based on the property names of your data model. However, you can customize the encoding and decoding process by implementing the `CodingKeys` enum in your Codable struct.

Let's modify our Person struct to include a custom `CodingKeys` enum to map the property names to different coding keys:

```swift
struct Person: Codable {
    var name: String
    var age: Int
    var address: String
    var job: Job
    
    enum CodingKeys: String, CodingKey {
        case name = "full_name"
        case age
        case address
        case job
    }
}
```

In this example, we map the property `name` to the coding key "full_name" instead. This allows you to customize the JSON representation of your data models to match the specific requirements of your API or data format.

## Conclusion

Codable is a powerful and convenient protocol in Swift that allows you to easily serialize and deserialize data models. By leveraging Codable, you can build flexible and extensible data models that can be encoded to and decoded from different data formats. Understanding the basics of Codable and how to customize the encoding and decoding process will enable you to work efficiently with data in Swift.

#Swift #Codable