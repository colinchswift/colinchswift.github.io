---
layout: post
title: "Using Swift Mirror to determine if an object is a class or a struct"
description: " "
date: 2023-09-21
tags: []
comments: true
share: true
---

Determining the type of an object at runtime can be useful in certain situations, especially in dynamically-typed languages like Swift. Luckily, Swift provides the `Mirror` class from the `Swift` module that allows us to inspect the properties and characteristics of an object.

In this blog post, we will explore how to use `Mirror` to determine if an object is a class or a struct in Swift.

## What is `Mirror`?

`Mirror` is a Swift class that provides a way to describe the structure and contents of a value or an instance of a type. It allows us to reflect upon an object's properties, methods, and other characteristics at runtime.

To use `Mirror`, we first need to import the `Swift` module. We can then create a mirror by initializing it with the object that we want to inspect.

## Checking if an Object is a Class or a Struct

To determine if an object is a class or a struct, we can use the `Mirror` class along with the `ValueType` property. The `ValueType` property of a mirror indicates whether the reflected subject is a struct or a class.

Here's an example code snippet that demonstrates how to check the type of an object:

```swift
import Swift

class MyClass {
    var property: String = "Hello, World!"
}

struct MyStruct {
    var property: String = "Hello, Struct!"
}

let object1 = MyClass()
let object2 = MyStruct()

let mirror1 = Mirror(reflecting: object1)
let mirror2 = Mirror(reflecting: object2)

if let valueType1 = mirror1.valueType {
    print("Object 1 is of type: \(valueType1)")
} else {
    print("Object 1 is not a class or struct")
}

if let valueType2 = mirror2.valueType {
    print("Object 2 is of type: \(valueType2)")
} else {
    print("Object 2 is not a class or struct")
}
```

In this example, we define a class `MyClass` and a struct `MyStruct`. We then create instances of these types: `object1` and `object2`. By using `Mirror(reflecting:)`, we create mirrors `mirror1` and `mirror2` to inspect these objects.

Finally, we check the `valueType` property of the mirrors to determine if the objects are classes or structs.

## Conclusion

In this blog post, we explored how to use `Mirror` in Swift to determine if an object is a class or a struct. By using the `ValueType` property, we can easily check the type of an object at runtime. This knowledge can be particularly useful in scenarios where we need to perform different operations based on the object's underlying type.

Remember to import the `Swift` module and create a mirror using the `Mirror(reflecting:)` initializer to start inspecting objects.