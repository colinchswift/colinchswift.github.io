---
layout: post
title: "Supporting multiple languages in Swift ResearchKit"
description: " "
date: 2023-09-25
tags: [ResearchKit, Localization]
comments: true
share: true
---

ResearchKit is a powerful and popular framework for building medical research apps on iOS. It provides a range of pre-built UI components and functionality to collect data from participants. When developing a research app, it is essential to consider supporting multiple languages to ensure accessibility and user-friendliness for a global audience.

In this blog post, we will discuss how to add multi-language support to your ResearchKit app using Swift. We will explore localization techniques, string translations, and best practices to create a seamless user experience.

## Localization in Swift

Localization is the process of adapting an application to a specific language or region. Swift provides excellent support for localization through its built-in `NSLocalizedString` API. This API allows us to retrieve translated strings based on the user's preferred language.

To begin, we need to prepare our app for localization. We can do this by marking localized strings in our code using the NSLocalizedString macro. For example:
```swift
let localizedString = NSLocalizedString("hello_world", comment: "Greeting")
```
In this example, the `hello_world` string is marked for localization, and a comment is added to provide context for translators.

## Creating Localization Files

To provide translations for different languages, we need to create `.strings` files for each supported language. These files contain key-value pairs, where the key corresponds to the string identifier, and the value is the translated string.

For example, to support English and French, we would create two separate `.strings` files: `Localizable.strings` and `Localizable.strings (French)`. The English version would contain:
```swift
"hello_world" = "Hello, World!";
```
While the French version would contain:
```swift
"hello_world" = "Bonjour le monde!";
```
We can then organize these `.strings` files into language-specific folders within our Xcode project.

## Switching Languages at Runtime

To switch between languages at runtime, we can use the `Bundle.setLanguage` extension method. This method overrides the preferred language and forces the app to load the appropriate `.strings` file for the selected language. Here's an example implementation:

```swift
extension Bundle {
    private static var bundleKey: UInt8 = 0
    
    class func setLanguage(_ language: String) {
        let onceToken = NSUUID().uuidString
        if Bundle.once(token: onceToken) {
            object_setClass(Bundle.main, PrivateBundle.self)
        }
        objc_setAssociatedObject(Bundle.main, &bundleKey, language as NSString? ?? "", objc_AssociationPolicy.OBJC_ASSOCIATION_RETAIN_NONATOMIC)
    }

    private static func once(token: String) -> Bool {
        let defaults = UserDefaults.standard
        if defaults.object(forKey: token) == nil {
            defaults.set(true, forKey: token)
            defaults.synchronize()
            return true
        }
        return false
    }
}

@objc private class PrivateBundle: Bundle {
    override func localizedString(forKey key: String, value: String?, table tableName: String?) -> String {
        let currentLanguage = objc_getAssociatedObject(self, &bundleKey) as? String
        let bundlePath = Bundle.main.path(forResource: currentLanguage, ofType: "lproj")
        let languageBundle = Bundle(path: bundlePath ?? "")
        return languageBundle?.localizedString(forKey: key, value: value, table: tableName) ?? super.localizedString(forKey: key, value: value, table: tableName)
    }
}
```

With this extension in place, we can easily switch the language by calling `Bundle.setLanguage("fr")` to set our app to French or `Bundle.setLanguage("en")` to switch it back to English.

## Conclusion

Supporting multiple languages in your Swift ResearchKit app is critical to reaching a global audience and ensuring accessibility. By leveraging localization and string translation techniques, you can create a user-friendly experience for users around the world. Swift's built-in support for localization makes it easy to implement multi-language support efficiently and effectively.

#ResearchKit #Localization