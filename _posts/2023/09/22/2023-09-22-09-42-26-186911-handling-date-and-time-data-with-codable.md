---
layout: post
title: "Handling date and time data with Codable"
description: " "
date: 2023-09-22
tags: [Codable]
comments: true
share: true
---

If you are working with date and time data in your Swift app and using Codable to serialize or deserialize your data, you may encounter some challenges. In this blog post, we will explore how to handle date and time data seamlessly with Codable.

## The Codable Protocol

The Codable protocol was introduced in Swift 4 and is a convenient way to convert your custom types to and from external representations, such as JSON or Property List. By conforming to the Codable protocol, you can simplify the process of encoding and decoding your data.

## Dealing with Dates

When dealing with date data, Codable provides a way to convert dates to and from a specific format.

To specify the format of your dates, you can use DateFormatter. Here's an example of how you can define a DateFormatter with a specific format:

```swift
let dateFormatter = DateFormatter()
dateFormatter.dateFormat = "yyyy-MM-dd"
```

Now, let's say you have a struct with a date property that needs to be encoded and decoded:

```swift
struct Event: Codable {
    let title: String
    let date: Date
}
```

To ensure that the date property is properly encoded and decoded, you can provide a custom coding strategy that uses the DateFormatter:

```swift
extension DateFormatter {
    static let customFormatter: DateFormatter = {
        let dateFormatter = DateFormatter()
        dateFormatter.dateFormat = "yyyy-MM-dd"
        return dateFormatter
    }()
}

extension Event {
    enum CodingKeys: String, CodingKey {
        case title
        case date
    }
    
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        title = try container.decode(String.self, forKey: .title)
        let dateString = try container.decode(String.self, forKey: .date)
        if let formattedDate = DateFormatter.customFormatter.date(from: dateString) {
            date = formattedDate
        } else {
            throw DecodingError.dataCorruptedError(forKey: .date, in: container, debugDescription: "Date string does not match format expected.")
        }
    }
    
    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(title, forKey: .title)
        let dateString = DateFormatter.customFormatter.string(from: date)
        try container.encode(dateString, forKey: .date)
    }
}
```

With this custom coding strategy, when you encode an instance of the Event struct, the date will be converted to a string using the specified date format. When you decode data, the string will be converted back to a Date object using the same format.

## Time Zones and Timestamps

If you need to include time zone information or work with timestamps, you can use the ISO 8601 format, which is a widely-supported standard for representing dates, times, and time zones. 

Swift provides a built-in ISO 8601 compliant date formatter. Here's an example of how you can create an ISO 8601 date formatter:

```swift
let isoDateFormatter = ISO8601DateFormatter()
isoDateFormatter.formatOptions = [.withInternetDateTime, .withFractionalSeconds]
```

By specifying the `.withInternetDateTime` option, the formatter will include the timestamp and time zone information in the resulting string.

You can then use this formatter similar to the custom formatter in the previous example to handle encoding and decoding date and time data using Codable.

## Conclusion

With Codable, handling date and time data in your Swift app becomes more convenient and straightforward. By providing custom coding strategies or using built-in formatters like DateFormatter or ISO8601DateFormatter, you can ensure that your date and time data is properly encoded and decoded. So go ahead and make your app's date and time data Codable!

#swift #Codable #dateandtime #ISO8601