---
layout: post
title: "Parsing JSON with weather data in Swift"
description: " "
date: 2023-10-24
tags: []
comments: true
share: true
---

In many iOS apps, we often need to display real-time weather data to the user. This data is commonly provided in JSON format by weather APIs. In this blog post, we will explore how to parse JSON with weather data in Swift.

## Table of Contents
- [Intro to JSON](#intro-to-json)
- [Parsing JSON in Swift](#parsing-json-in-swift)
- [Accessing Weather Data](#accessing-weather-data)
- [Displaying Weather Data](#displaying-weather-data)
- [Conclusion](#conclusion)

## Intro to JSON

JSON (JavaScript Object Notation) is a lightweight data-interchange format that is easy to read and write for humans. It is widely used for structured data transmission between a server and a web/mobile application.

## Parsing JSON in Swift

Swift provides built-in support for parsing JSON data using the `Codable` protocol. The `Codable` protocol allows you to decode and encode data types to and from JSON. To parse JSON with weather data, you need to define a struct that conforms to the `Codable` protocol and has properties corresponding to the JSON keys.

```swift
struct WeatherData: Codable {
    let temperature: Double
    let description: String
    // Add other properties as needed
}
```

## Accessing Weather Data

To fetch weather data from a weather API, you can use URLSession to make a network request. Once you receive the response, you can parse the JSON data using the `JSONDecoder`.

```swift
let url = URL(string: "http://api.weatherapi.com/v1/current.json?key=YOUR_API_KEY&q=New%20York")!
let task = URLSession.shared.dataTask(with: url) { (data, response, error) in
    if let error = error {
        print("Error: \(error)")
    } else if let data = data {
        do {
            let weatherData = try JSONDecoder().decode(WeatherData.self, from: data)
            // Process the weather data
        } catch {
            print("Error decoding JSON: \(error)")
        }
    }
}
task.resume()
```

Make sure to replace `YOUR_API_KEY` with your actual API key and the `q` parameter with the desired location.

## Displaying Weather Data

Once you have parsed the weather data, you can use it to update your UI. For example, you can display the temperature and description in labels on a weather app's main screen.

```swift
let temperatureLabel = UILabel()
let descriptionLabel = UILabel()

// Update the labels with the parsed weather data
temperatureLabel.text = "\(weatherData.temperature)°C"
descriptionLabel.text = weatherData.description
```

## Conclusion

Parsing JSON with weather data in Swift is made easy with the Codable protocol. By defining a struct that conforms to Codable, you can parse JSON data effortlessly. Remember to handle errors and update your UI based on the parsed weather data.

Happy coding! ☀️

**References**:
- [Apple Documentation - JSONEncoder](https://developer.apple.com/documentation/foundation/jsonencoder)
- [Apple Documentation - JSONDecoder](https://developer.apple.com/documentation/foundation/jsondecoder)