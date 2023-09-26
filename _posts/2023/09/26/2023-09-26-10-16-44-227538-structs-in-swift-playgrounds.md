---
layout: post
title: "Structs in Swift Playgrounds"
description: " "
date: 2023-09-26
tags: [SwiftPlaygrounds, Structs]
comments: true
share: true
---

## Introduction
In Swift, structs are an essential data structure that allows you to encapsulate related properties and behavior. They provide a way to define custom types, similar to classes, but with some important differences. In this blog post, we will explore how to use structs in Swift Playgrounds, harnessing their power to create reusable and modular code.

## Creating a Struct
Creating a struct is fairly straightforward in Swift Playgrounds. You start by using the `struct` keyword, followed by the name of your struct. Let's create a simple `Person` struct as an example:

```swift
struct Person {
    var name: String
    var age: Int
}
```

In this example, we defined a `Person` struct with two properties: `name` of type `String`, and `age` of type `Int`. 

## Initializing a Struct
Once we have defined our struct, we can create instances of it by using the initializer. The initializer is invoked when we create a new object of a struct. Here's an example:

```swift
let person1 = Person(name: "John", age: 25)
```

In this code snippet, we created a new `Person` instance called `person1` and assigned it a name of "John" and an age of 25.

## Accessing Struct Properties
After we create an instance of a struct, we can access its properties using dot notation. Let's see how we can access the properties of our `person1` instance:

```swift
print(person1.name) // Output: "John"
print(person1.age) // Output: 25
```

## Mutating Struct Properties
By default, struct properties are immutable. To change the values of a struct's properties, we need to mark the instance as `var` instead of `let`. This is because `var` indicates that the instance can be modified. Let's see an example:

```swift
var person2 = Person(name: "Sarah", age: 30)
person2.age = 35 // Modifying the age property
```

In this code snippet, we created a `Person` instance called `person2` and assigned it a name of "Sarah" and an age of 30. Later, we modified the `age` property to 35.

## Conclusion
Structs in Swift Playgrounds allow us to define custom types with properties and behavior. They offer a powerful way to organize our code and create reusable components. By harnessing the power of structs, we can write modular and maintainable code. Now that you have a better understanding of structs in Swift Playgrounds, go ahead and try building your own custom types! #SwiftPlaygrounds #Structs