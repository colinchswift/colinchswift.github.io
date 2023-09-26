---
layout: post
title: "Supporting multi-language localization with JSONEncoder and JSONDecoder in Swift"
description: " "
date: 2023-09-27
tags: [Swift, Localization]
comments: true
share: true
---

In many software applications, supporting multiple languages and allowing users to switch between them is essential. When it comes to serializing and deserializing localized strings, JSON encoding and decoding can be a powerful tool.

In Swift, we can achieve multi-language localization using `JSONEncoder` and `JSONDecoder`, which are available as part of the Foundation framework. Here's an example of how we can serialize and deserialize localized strings using these classes.

## Localization setup

First, let's set up the localization in our project. Create a `Localizable.strings` file and add translations for all the strings you want to localize. For example:

```
/* English */
"welcome_message" = "Welcome to our app!";

/* French */
"welcome_message" = "Bienvenue dans notre application !";
```

Make sure to add translations for all the languages your app supports.

## Serialization

To serialize a localized string, we need to create a custom `CodingKey` enum that will map the keys defined in our `Localizable.strings` file. Here's an example:

```swift
enum LocalizationKey: String, CodingKey {
    case welcomeMessage = "welcome_message"
}
```

Next, we create a struct that represents our localized string:

```swift
struct LocalizedString: Codable {
    let message: String

    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: LocalizationKey.self)
        message = try container.decode(String.self, forKey: .welcomeMessage)
    }

    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: LocalizationKey.self)
        try container.encode(message, forKey: .welcomeMessage)
    }
}
```

In the `init(from:)` method, we decode the localized string by using the appropriate key from our `LocalizationKey` enum. In the `encode(to:)` method, we encode the localized string back by using the same key.

## Usage

To use our `LocalizedString` struct, we can simply encode and decode it using `JSONEncoder` and `JSONDecoder`:

```swift
let localizedString = LocalizedString(message: NSLocalizedString("welcome_message", comment: ""))

do {
    let encoder = JSONEncoder()
    let data = try encoder.encode(localizedString)

    // Store or send the serialized data

    let decoder = JSONDecoder()
    let decodedLocalizedString = try decoder.decode(LocalizedString.self, from: data)

    NSLog(decodedLocalizedString.message)
} catch {
    NSLog("Serialization/Deserialization error: \(error.localizedDescription)")
}
```

Here, we create an instance of `LocalizedString` with the localized string retrieved using `NSLocalizedString` function. We then encode it using `JSONEncoder` and save or send the serialized data. Finally, we decode the data using `JSONDecoder` and retrieve the deserialized localized string.

#Swift #Localization