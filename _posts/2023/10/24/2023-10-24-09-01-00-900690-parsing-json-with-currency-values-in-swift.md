---
layout: post
title: "Parsing JSON with currency values in Swift"
description: " "
date: 2023-10-24
tags: []
comments: true
share: true
---

One common task in Swift development is parsing JSON data received from a server. JSON (JavaScript Object Notation) is a popular data interchange format that is widely used in web applications.

In this tutorial, we will explore how to parse JSON data in Swift, specifically when dealing with currency values. We will assume that we have a JSON response that includes currency values in the following format:

```json
{
  "usd": {
    "symbol": "$",
    "value": 42.50
  },
  "eur": {
    "symbol": "€",
    "value": 37.20
  },
  "gbp": {
    "symbol": "£",
    "value": 30.90
  }
}
```

## Step 1: Fetching the JSON Data

First, we need to fetch the JSON data from the API endpoint or any other source. There are various ways of doing this in Swift, such as using URLSession, Alamofire, or other networking libraries. For this tutorial, we will use the URLSession API.

```swift
guard let url = URL(string: "https://api.example.com/currencies") else {
    // handle invalid URL
    return
}

let task = URLSession.shared.dataTask(with: url) { (data, response, error) in
    // handle the response
}
task.resume()
```

## Step 2: Parsing the JSON Data

Once we have received the JSON data, we can start parsing it. In Swift, we can use the `JSONSerialization` class to parse the JSON data into a Swift data structure.

```swift
guard let data = data else {
    // handle empty or invalid data
    return
}

do {
    let json = try JSONSerialization.jsonObject(with: data, options: [])
    if let currencies = json as? [String: Any] {
        for (key, value) in currencies {
            if let currency = value as? [String: Any],
               let symbol = currency["symbol"] as? String,
               let value = currency["value"] as? Double {
                // parse the currency values
                print("Currency: \(key), Symbol: \(symbol), Value: \(value)")
            }
        }
    }
} catch {
    // handle parsing error
}
```

## Step 3: Using the Parsed Currency Values

Now that we have parsed the currency values, we can use them in our application. For example, we can display the values in a UITableView or use them for further calculations.

```swift
// Assuming we have a UITableView named "tableView"
var currencyValues: [String: Double] = [:]

// Inside the parsing loop from Step 2
currencyValues[key] = value

// UITableViewDataSource methods

func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    return currencyValues.count
}

func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    let cell = tableView.dequeueReusableCell(withIdentifier: "CurrencyCell", for: indexPath)
    
    let currency = Array(currencyValues.keys)[indexPath.row]
    let value = currencyValues[currency]
    
    cell.textLabel?.text = currency
    cell.detailTextLabel?.text = "$\(value ?? 0.00)"
    
    return cell
}
```

In this example, we are storing the parsed currency values in a dictionary named `currencyValues`. We then use these values to populate the cells of a UITableView.

## Conclusion

Parsing JSON data, especially when dealing with currency values, is an essential task in Swift development. By using the `JSONSerialization` class, we can easily parse JSON data into Swift data structures and use that data in our applications.

Throughout this tutorial, we covered the basic steps of fetching and parsing JSON data in Swift, as well as using the parsed currency values in a UITableView. By understanding these principles, you will be better equipped to handle different JSON data formats and integrate them into your Swift projects.

# References
- [JSONSerialization - Apple Developer Documentation](https://developer.apple.com/documentation/foundation/jsonserialization)
- [URLSession - Apple Developer Documentation](https://developer.apple.com/documentation/foundation/urlsession)
- [Alamofire - GitHub Repository](https://github.com/Alamofire/Alamofire)