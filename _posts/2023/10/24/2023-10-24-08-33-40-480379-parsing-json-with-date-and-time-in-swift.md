---
layout: post
title: "Parsing JSON with date and time in Swift"
description: " "
date: 2023-10-24
tags: []
comments: true
share: true
---

In Swift, parsing JSON with date and time values can be a bit tricky due to the need to handle different date formats and timezones. However, with the help of built-in classes and some additional code, parsing JSON with date and time can be easily accomplished. 

Let's go through the steps to parse JSON with date and time in Swift.

## 1. Define a Decodable Struct

First, define a Decodable struct that represents the JSON response you expect to receive. Make sure to add properties for the date and time values. For example:

```swift
struct Event: Decodable {
    let title: String
    let dateTime: Date
}
```

## 2. Implement the Decodable Protocol

To parse the JSON data into your custom struct, you need to implement the Decodable protocol. Conform to Decodable and provide a CodingKeys enum to specify the keys to be used for mapping the JSON properties. 

```swift
enum CodingKeys: String, CodingKey {
    case title
    case dateTime = "date_time"
}
```

## 3. Decode the JSON Data

To decode the JSON data, use the JSONDecoder class provided by the Foundation framework. Set the `dateDecodingStrategy` property of the decoder to handle the date and time formats correctly.

```swift
let decoder = JSONDecoder()
decoder.dateDecodingStrategy = .iso8601 // or any other appropriate date formatting, e.g. .formatted(dateFormatter)

do {
    let event = try decoder.decode(Event.self, from: jsonData)
    print(event.title)
    print(event.dateTime)
} catch {
    print("Error decoding JSON: \(error)")
}
```

In the above code, `jsonData` is the JSON data returned from the API call.

## 4. Handle Different Date Formats

By default, JSONDecoder uses the ISO 8601 date format to decode dates. However, if the API returns dates in a different format, you can provide a custom date formatter and set the `dateDecodingStrategy` accordingly.

```swift
let decoder = JSONDecoder()
let dateFormatter = DateFormatter()
dateFormatter.dateFormat = "yyyy-MM-dd'T'HH:mm:ssZ" // Customize the date format according to your JSON response
decoder.dateDecodingStrategy = .formatted(dateFormatter)
```

Make sure to modify the date format to match the format used in your JSON response.

## Conclusion

Parsing JSON with date and time values in Swift can be achieved by properly implementing the Decodable protocol and setting the correct date decoding strategy. By following the steps outlined in this article, you should be able to parse date and time values from JSON responses easily. 

Remember to handle different date formats and timezones if they vary in your application. Happy coding!

# References
- [Apple Developer Documentation on Decodable](https://developer.apple.com/documentation/swift/decodable)
- [Apple Developer Documentation on JSONDecoder](https://developer.apple.com/documentation/foundation/jsondecoder)