---
layout: post
title: "Decoding JSON into Swift enums"
description: " "
date: 2023-09-27
tags: [JSON]
comments: true
share: true
---

In Swift, enums provide a powerful way to represent a fixed set of related values. Enums can be particularly useful when working with JSON data, as they allow you to map the different cases of an enum to the corresponding JSON values.

In this blog post, we will explore the process of decoding JSON into Swift enums using the Codable protocol. Codable is a protocol introduced in Swift 4 that allows you to easily convert between Swift types and external representations, such as JSON.

## Creating the JSON structure

To begin, let's start by defining a simple JSON structure that we want to decode into Swift enums. Imagine we have a JSON response from a weather API that looks like this:

```json
{
  "temperature": 25,
  "weather": "sunny"
}
```

Our goal is to create a Swift struct that represents this JSON structure and converts the "weather" field into an enum.

## Defining the Swift struct

First, let's define a Swift struct called WeatherData that conforms to the Codable protocol:

```swift
struct WeatherData: Codable {
    let temperature: Int
    let weather: Weather
}
```

In this struct, we have two properties: temperature of type Int, and weather of type Weather. The Weather type is the enum that we will define next.

## Defining the Swift enum

Now let's define the Weather enum, which will represent the different weather conditions:

```swift
enum Weather: String, Codable {
    case sunny
    case cloudy
    case rainy
    case snowy
}
```

In this enum, each case corresponds to a possible weather condition. We have defined four cases: sunny, cloudy, rainy, and snowy. By explicitly specifying the raw value as a string for each case, we can easily map the JSON value to the corresponding enum case.

## Decoding the JSON

To decode the JSON into Swift enums, we will use the JSONDecoder class provided by Swift. Here's an example of how we can decode the JSON into an instance of the WeatherData struct:

```swift
let json = """
{
  "temperature": 25,
  "weather": "sunny"
}
"""

let jsonData = json.data(using: .utf8)!
let decoder = JSONDecoder()

do {
    let weatherData = try decoder.decode(WeatherData.self, from: jsonData)
    print(weatherData.weather) // Output: sunny
} catch {
    print("Error decoding JSON: \(error)")
}
```

In this code snippet, we first convert the JSON string into Data using the utf8 encoding. Then, we create an instance of JSONDecoder and call its decode method, passing in the WeatherData.self type and the JSON data. The result is an instance of WeatherData struct with the decoded values.

## Conclusion

Decoding JSON into Swift enums is a convenient way to represent and handle fixed sets of values. By leveraging the Codable protocol and JSONDecoder class, you can easily convert JSON data into Swift enums. Enums provide type safety and help ensure that we handle all possible cases in our code.

If you're working with more complex JSON structures, you can extend this approach to decode nested enums or enums with associated values. Codable provides flexibility and allows you to adapt to different JSON structures while keeping your code clean and easy to read.

#Swift #JSON #enum