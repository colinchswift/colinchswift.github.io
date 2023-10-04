---
layout: post
title: "Encoding JSON with date values in Swift"
description: " "
date: 2023-09-27
tags: [JSON]
comments: true
share: true
---

Working with JSON in Swift often requires encoding and decoding data from and to JSON format. In some cases, you may need to include date values in your JSON data. In this blog post, we will explore how to encode date values into a JSON payload using Swift.

To begin with, let's assume we have a struct or class that represents our data model, which includes a `Date` property:

```swift
struct MyDataModel: Codable {
    let id: String
    let name: String
    let dateCreated: Date
}
```

To encode this data model into JSON with the `dateCreated` property, we need to create an instance of `JSONEncoder` and set its `dateEncodingStrategy` property to the desired encoding strategy. For encoding `Date` values into JSON, we can use the `.iso8601` strategy, which formats the date as a string in an ISO 8601-compliant format.

```swift
let dataModel = MyDataModel(id: "123", name: "Example", dateCreated: Date())

let encoder = JSONEncoder()
encoder.dateEncodingStrategy = .iso8601

do {
    let jsonData = try encoder.encode(dataModel)
    let jsonString = String(data: jsonData, encoding: .utf8)
    print(jsonString)
} catch {
    print("Error encoding JSON: \(error.localizedDescription)")
}
```

In the above code, we create an instance of `MyDataModel` with some example values, and then create an instance of `JSONEncoder`. We set the `dateEncodingStrategy` of the encoder to `.iso8601` to ensure that the `Date` property is encoded as a string in ISO 8601 format.

Upon encoding the data model, we convert the resulting `Data` object into a `String` using the `.utf8` encoding. Finally, we print the JSON string representation to the console.

The output should be similar to:

```json
{"id":"123","name":"Example","dateCreated":"2021-10-20T12:34:56Z"}
```

By default, the `JSONEncoder` uses the ISO 8601 format for encoding date values, making it a convenient choice for most scenarios. However, you can also specify other encoding strategies depending on your requirements.

Remember to import the `Foundation` framework to use the `JSONEncoder` and `Date` classes in your Swift file.

That's it! Now you know how to encode date values into JSON using Swift. You can use this knowledge to work with date properties and encode them correctly in your JSON payloads. Happy coding!

#Swift #JSON #DateEncoding