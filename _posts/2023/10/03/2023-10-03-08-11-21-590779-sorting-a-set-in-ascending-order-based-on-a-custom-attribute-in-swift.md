---
layout: post
title: "Sorting a set in ascending order based on a custom attribute in Swift"
description: " "
date: 2023-10-03
tags: []
comments: true
share: true
---

Let's say you have a set of objects with a custom attribute called `score`, and you want to sort them in ascending order based on this attribute. Here's an example code snippet that demonstrates how to perform this sorting:

```swift
struct CustomObject {
    let name: String
    let score: Int
}

let object1 = CustomObject(name: "Object 1", score: 10)
let object2 = CustomObject(name: "Object 2", score: 5)
let object3 = CustomObject(name: "Object 3", score: 7)

var objectSet: Set<CustomObject> = [object1, object2, object3]

let sortedSet = objectSet.sorted { $0.score < $1.score }

for object in sortedSet {
    print("Name: \(object.name), Score: \(object.score)")
}
```

In the code above, we define a struct `CustomObject` that represents objects with a `name` and `score`. We create three instances of `CustomObject` and add them to a set called `objectSet`.

To sort the set based on the `score` attribute, we use the `sorted(by:)` method and provide a closure that compares the `score` attributes of two objects. The closure returns `true` if the first object's `score` is less than the second object's `score`.

Finally, we iterate over the `sortedSet` and print the name and score of each object. The output will be:

```
Name: Object 2, Score: 5
Name: Object 3, Score: 7
Name: Object 1, Score: 10
```

By using the `sorted(by:)` method and providing a closure that defines the sorting rules, you can easily sort a set based on a custom attribute in Swift.