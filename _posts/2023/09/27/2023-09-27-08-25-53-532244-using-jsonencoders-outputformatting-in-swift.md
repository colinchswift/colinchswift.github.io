---
layout: post
title: "Using JSONEncoder's outputFormatting in Swift"
description: " "
date: 2023-09-27
tags: [Swift, JSONEncoderFormatting]
comments: true
share: true
---

In Swift, JSONEncoder is a powerful tool for encoding Swift objects into JSON data. It provides various options and parameters to customize the encoding process, one of which is the `outputFormatting` property. This property allows you to control the formatting of the output JSON data.

By default, the `outputFormatting` property of JSONEncoder is set to `.prettyPrinted`, which means the generated JSON data will be nicely formatted with indentation and line breaks. However, if you want to customize the output formatting, you can override this property with different options.

Here's an example of how to use the `outputFormatting` property in JSONEncoder:

```swift
import Foundation

struct Person: Codable {
    var name: String
    var age: Int
}

let person = Person(name: "John Doe", age: 30)
let encoder = JSONEncoder()

// Set the output formatting to .sortedKeys
encoder.outputFormatting = .sortedKeys

do {
    let jsonData = try encoder.encode(person)
    if let jsonString = String(data: jsonData, encoding: .utf8) {
        print(jsonString)
    }
} catch {
    print("Encoding failed: \(error.localizedDescription)")
}
```

In the above example, we create a `Person` struct with a `name` and `age` property. We then create an instance of `JSONEncoder` and set the `outputFormatting` property to `.sortedKeys`. This option ensures that the keys in the JSON data are sorted alphabetically.

Finally, we encode the `person` object into JSON data using the encoder, and convert it to a string for printing. When we run the code, the output will be a sorted JSON string:

```
{\"age\":30,\"name\":\"John Doe\"}
```

By customizing the `outputFormatting` property, you can have more control over how the JSON data is formatted. Other options available include `.prettyPrinted` for the default formatting, `.withoutEscapingSlashes` to omit escaping slashes, and `.withoutWhitespace` to remove all whitespace.

#Swift #JSONEncoderFormatting