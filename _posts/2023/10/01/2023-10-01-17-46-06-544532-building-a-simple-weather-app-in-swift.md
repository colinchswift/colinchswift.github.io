---
layout: post
title: "Building a simple weather app in Swift"
description: " "
date: 2023-10-01
tags: [swift, weather]
comments: true
share: true
---

In this blog post, we will walk through the steps to build a simple weather app using Swift programming language. This app will fetch weather data from an API and display it to the user. Let's get started!

## Prerequisites
Before we begin, make sure you have the following prerequisites:

1. Xcode installed on your Mac
2. Basic understanding of Swift programming language
3. Knowledge of working with APIs

## Step 1: Create a new Xcode project
Open Xcode and create a new single-view application project. Choose a suitable name and bundle identifier for your app.

## Step 2: Design the User Interface
In the storyboard, design the user interface of the app. Add a text field to enter the city name and a button to fetch the weather data. Also, add labels to display the weather information such as temperature, humidity, and description.

## Step 3: Fetch weather data from API
In your view controller code, create an IBAction for the fetch weather button. Inside the IBAction function, use URLSession to make a GET request to the weather API, providing the city name as a parameter.

Here is an example code snippet to fetch weather data:

```swift
@IBAction func fetchWeather(_ sender: UIButton) {
    let cityName = cityNameTextField.text ?? ""
    let urlString = "https://api.weatherapi.com/v1/current.json?key=YOUR_API_KEY&q=\(cityName)"
    guard let url = URL(string: urlString) else { return }

    URLSession.shared.dataTask(with: url) { (data, _, error) in
        if let error = error {
            print("Error: \(error.localizedDescription)")
            return
        }
        
        guard let data = data else { return }
        
        do {
            let weatherData = try JSONDecoder().decode(WeatherData.self, from: data)
            
            // Update UI with weather data
            DispatchQueue.main.async {
                self.temperatureLabel.text = "\(weatherData.current.tempC)°C"
                self.humidityLabel.text = "\(weatherData.current.humidity)%"
                self.descriptionLabel.text = weatherData.current.condition.text
            }
        } catch {
            print("Error decoding weather data: \(error.localizedDescription)")
        }
    }.resume()
}
```

## Step 4: Parsing the JSON response
To parse the JSON response from the weather API, create a struct that conforms to the Codable protocol. This struct should have properties representing the desired weather information.

Here is an example struct for weather data:

```swift
struct WeatherData: Codable {
    struct Current: Codable {
        let tempC: Double
        let humidity: Double
        let condition: Condition
        
        struct Condition: Codable {
            let text: String
        }
    }
    
    let current: Current
}
```

## Step 5: Run the app
Now, build and run your app on a simulator or a physical device. Enter a city name in the text field and tap the fetch weather button. The app should fetch weather data from the API and display it on the screen.

Congratulations! You have successfully built a simple weather app in Swift.

## Conclusion
In this blog post, we learned how to build a simple weather app using Swift. We covered the steps to create a new Xcode project, design the user interface, fetch weather data from an API, parse the JSON response, and display the data on the screen.

Stay tuned for more exciting Swift tutorials!

\#swift \#weather-app