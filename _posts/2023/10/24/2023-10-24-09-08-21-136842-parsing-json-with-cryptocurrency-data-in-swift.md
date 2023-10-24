---
layout: post
title: "Parsing JSON with cryptocurrency data in Swift"
description: " "
date: 2023-10-24
tags: [JSON, Cryptocurrency]
comments: true
share: true
---

In this tutorial, we will explore how to parse JSON data containing cryptocurrency information using Swift programming language. JSON (JavaScript Object Notation) is a popular format for exchanging data. Cryptocurrency data, such as prices, market caps, and trading volumes, is often available in JSON format through various APIs.

## 1. Understanding the JSON Data

Before we start parsing the JSON data, let's first understand its structure. Cryptocurrency data typically contains an array of objects, where each object represents a specific cryptocurrency and its details. Some common properties include name, symbol, price, market cap, and volume.

Here is a sample JSON data representing cryptocurrency information:

```json
{
  "data": [
    {
      "name": "Bitcoin",
      "symbol": "BTC",
      "price": 60989.08,
      "market_cap": 1141128960794,
      "volume_24h": 47413271140
    },
    {
      "name": "Ethereum",
      "symbol": "ETH",
      "price": 3846.21,
      "market_cap": 448042525421,
      "volume_24h": 32445891394
    },
    ...
  ]
}
```

## 2. Setting up URLSession

To fetch the JSON data from an API, we will use URLSession. Here is an example function that sets up a URLSession and fetches the JSON data from a given URL:

```swift
func fetchCryptocurrencyData(completion: @escaping ([Cryptocurrency]?) -> Void) {
    guard let url = URL(string: "https://api.example.com/cryptocurrency-data") else {
        completion(nil)
        return
    }
    
    URLSession.shared.dataTask(with: url) { (data, response, error) in
        if let error = error {
            print("Error fetching data: \(error.localizedDescription)")
            completion(nil)
            return
        }
        
        if let data = data {
            do {
                let jsonDecoder = JSONDecoder()
                let cryptocurrencies = try jsonDecoder.decode([Cryptocurrency].self, from: data)
                completion(cryptocurrencies)
            } catch {
                print("Error decoding JSON: \(error.localizedDescription)")
                completion(nil)
            }
        } else {
            completion(nil)
        }
    }.resume()
}
```

In this function, we first create a URL object using the API's URL. Then, we use URLSession.shared.dataTask(with:completionHandler:) method to fetch the JSON data asynchronously. Inside the completion handler, we handle any potential errors and decode the received data into an array of `Cryptocurrency` objects using JSONDecoder.

## 3. Creating the Cryptocurrency Model

To represent each cryptocurrency object, we need to create a model. Create a new Swift file called `Cryptocurrency.swift` and define the `Cryptocurrency` struct as follows:

```swift
struct Cryptocurrency: Codable {
    let name: String
    let symbol: String
    let price: Double
    let marketCap: Double
    let volume24h: Double
    
    enum CodingKeys: String, CodingKey {
        case name
        case symbol
        case price
        case marketCap = "market_cap"
        case volume24h = "volume_24h"
    }
}
```

The `Codable` protocol is used to enable encoding and decoding of the struct from/to JSON. We also define `codingKeys` enum to match the JSON keys with the struct properties.

## 4. Parsing the JSON Data

Now that we have the model and a function to fetch the JSON data, let's parse it into an array of `Cryptocurrency` objects. Here's an example of how to use the `fetchCryptocurrencyData` function and parse the JSON response:

```swift
fetchCryptocurrencyData { cryptocurrencies in
    if let cryptocurrencies = cryptocurrencies {
        // JSON data parsed successfully
        for cryptocurrency in cryptocurrencies {
            print("Name: \(cryptocurrency.name)")
            print("Symbol: \(cryptocurrency.symbol)")
            print("Price: \(cryptocurrency.price)")
            print("Market Cap: \(cryptocurrency.marketCap)")
            print("Volume (24h): \(cryptocurrency.volume24h)")
            print("---")
        }
    } else {
        // Error occurred or JSON data couldn't be parsed
        print("Failed to fetch or parse cryptocurrency data.")
    }
}
```

In this example, we call the `fetchCryptocurrencyData` function and handle the retrieved cryptocurrencies. We simply print some of the cryptocurrency details to the console, but you can use the parsed data in any other way based on your application's requirements.

## Summary

In this tutorial, we've learned how to parse JSON data containing cryptocurrency information in Swift. We explored how to fetch the JSON data using URLSession, create a model for the cryptocurrency objects, and parse the JSON response into an array of `Cryptocurrency` objects. This understanding can be applied to various other JSON parsing scenarios in Swift.

If you're interested in diving deeper into JSON parsing, you can check out the [official documentation](https://developer.apple.com/documentation/foundation/archives_and_serialization) provided by Apple.

#hashtags #Swift #JSON #Cryptocurrency