---
layout: post
title: "Handling localization and multilingual support with Codable in Swift"
description: " "
date: 2023-09-22
tags: [Swift, Localization]
comments: true
share: true
---

As mobile applications continue to expand their reach globally, enforcing support for multiple languages has become crucial. In the case of iOS app development using Swift, Apple's built-in localization support simplifies the process considerably. However, when it comes to handling localized data models and JSON encoding/decoding, leveraging Swift's Codable protocol can provide an elegant solution. In this article, we will explore how to handle localization and multilingual support with Codable in Swift.

## Creating Localized Data Models

To handle localization and multilingual data in Swift, we need to create localized data models. These models represent the structure of the data that varies across different languages. Let's consider an example where we have a `Product` model with name and description attributes that need to be localized.

```swift
struct Product: Codable {
    var name: LocalizedString
    var description: LocalizedString
}

struct LocalizedString: Codable {
    var values: [String: String]
}
```

In the above example, we introduce a `LocalizedString` struct that holds a dictionary of translations, where the key is the language code and the value is the translation for that language. By encapsulating the translations within a separate struct, we can easily extend it to include additional localized attributes in the future.

## Implementing Codable for Localization

To enable proper encoding and decoding of these localized data models, we need to implement the `Codable` protocol in Swift. The `LocalizedString` struct can conform to `Codable` since it only contains a dictionary, which itself conforms to `Codable` due to its simple key-value structure.

```swift
struct LocalizedString: Codable {
    var values: [String: String]

    enum CodingKeys: String, CodingKey {
        case values
    }

    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        values = try container.decode([String: String].self, forKey: .values)
    }

    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(values, forKey: .values)
    }
}
```

With this implementation, we can seamlessly encode and decode localized data models without any additional configuration.

## Managing Localization Resources

To simplify the management of localized strings, we can utilize `.strings` file for each language. In Xcode, go to *File* -> *New* -> *File*, then select *Strings File* from the templates. Name the file as `Localizable.strings` and add a name-value pair as follows:

```swift
// Localizable.strings (English)
"product_name" = "Product";
"product_description" = "This is a product";
```

```swift
// Localizable.strings (French)
"product_name" = "Produit";
"product_description" = "C'est un produit";
```

You can create separate `.strings` files for each language your app supports, where the keys correspond to the attributes in the localized data model.

## Utilizing Localization in Coding

Now, let's see how we can leverage the localized data models and the `.strings` files to fetch the localized values programmatically.

```swift
// Retrieve the localized string for a specific attribute
func getLocalizedString(for attribute: String) -> String {
    guard let localizedString = product.name.values[Locale.current.languageCode ?? ""] else {
        return ""
    }
    return localizedString
}

// Fetch localized values from the .strings files
let productName = getLocalizedString(for: "product_name")
let productDescription = getLocalizedString(for: "product_description")
```

In the above example, we retrieve the localized string for a given language code (e.g., "en" for English) from the `values` dictionary. We can obtain the current language code using `Locale.current.languageCode`.

## Conclusion

In this article, we explored how to handle localization and multilingual support with Codable in Swift. By creating localized data models, implementing Codable for localization, and managing localization resources, we can seamlessly encode and decode localized data models. Leveraging the power of Codable and Swift's built-in localization support helps us create robust and multilingual applications efficiently. #Swift #Localization