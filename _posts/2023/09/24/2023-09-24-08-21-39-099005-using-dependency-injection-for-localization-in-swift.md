---
layout: post
title: "Using dependency injection for localization in Swift"
description: " "
date: 2023-09-24
tags: [localization]
comments: true
share: true
---

Localization is an important aspect of building multilingual applications. In swift, we can use dependency injection to efficiently manage and handle the localization process. Dependency injection allows us to decouple our dependencies and easily switch between different implementations.

## Setting up Localization

The first step is to set up localization in your project. This involves creating a `Localizable.strings` file for each language you want to support. In each file, you define key-value pairs for the translated strings. For example, in English:

```
"welcome_message" = "Welcome!";
```

And in French:

```
"welcome_message" = "Bienvenue!";
```

## Creating a Localization Service

Next, we'll create a `LocalizationService` class that will handle the retrieval of localized strings. This class will be responsible for loading the appropriate `Localizable.strings` file based on the current app language.

```swift
class LocalizationService {
    private let bundle: Bundle
    
    init(bundle: Bundle = .main) {
        self.bundle = bundle
    }
    
    func localizedString(for key: String) -> String {
        return bundle.localizedString(forKey: key, value: nil, table: "Localizable")
    }
}
```

## Injecting the Localization Service

Now that we have our `LocalizationService`, we can inject it into our view controllers or other components that need localized strings. This allows us to easily switch between different implementations or mock the service for testing purposes.

```swift
class ViewController: UIViewController {
    private let localizationService: LocalizationService
    
    init(localizationService: LocalizationService = LocalizationService()) {
        self.localizationService = localizationService
        super.init(nibName: nil, bundle: nil)
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let welcomeMessage = localizationService.localizedString(for: "welcome_message")
        // Display welcomeMessage in the UI
    }
    
    // Rest of the view controller code...
}
```

## Registering the Dependency

To ensure that the `LocalizationService` is properly injected into our view controllers, we need to register it in our dependency injection container. Depending on the dependency injection framework you're using, the process may vary. However, the idea is to register the `LocalizationService` as a dependency and then provide it when instantiating view controllers.

```swift
let container = DependencyContainer()
container.register(LocalizationService.self) { _ in LocalizationService() }
// Register other dependencies...
```

## Conclusion

Using dependency injection for localization in Swift can greatly improve the flexibility and maintainability of your codebase. By decoupling the localization service from the view controllers, we can easily switch between different implementations and mock the service for testing. This approach helps in building scalable and localized applications.

#swift #localization