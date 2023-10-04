---
layout: post
title: "Using async/await with biometrics authentication in Swift"
description: " "
date: 2023-10-04
tags: [biometrics, Swift]
comments: true
share: true
---

Biometrics authentication, such as Touch ID or Face ID, has become a popular method for user authentication in iOS apps. With the introduction of `async/await` in Swift 5.5, we can now write more concise and readable asynchronous code. In this blog post, we will explore how to use `async/await` with biometrics authentication in Swift.

## Adding Biometrics Authentication

To add biometrics authentication to your iOS app, you need to import the `LocalAuthentication` framework and create an instance of `LAContext`.

```swift
import LocalAuthentication

let context = LAContext()
```

## Checking Biometrics Availability

Before attempting biometrics authentication, it's a good practice to check if the device supports biometrics and if the user has enrolled any biometric data.

```swift
let biometricsAvailable = context.canEvaluatePolicy(.deviceOwnerAuthenticationWithBiometrics, error: nil)

if biometricsAvailable {
    // Biometrics is available, proceed with authentication
} else {
    // Biometrics is not available on the device
}
```

## Authenticating with Biometrics using async/await

Traditionally, biometrics authentication is performed using a completion handler. With `async/await`, we can now perform biometrics authentication in a more linear and readable way.

Here's an example of using `async/await` with biometrics authentication:

```swift
func authenticateWithBio() async throws {
    let biometricsAvailable = context.canEvaluatePolicy(.deviceOwnerAuthenticationWithBiometrics, error: nil)
    
    guard biometricsAvailable else {
        throw NSError(domain: "com.yourapp", code: -1, userInfo: [NSLocalizedDescriptionKey: "Biometrics authentication not available."])
    }
    
    do {
        let result = try await context.evaluatePolicy(.deviceOwnerAuthenticationWithBiometrics, localizedReason: "Authenticate to access your account.")
        
        if result {
            // Biometrics authentication successful
        } else {
            // Biometrics authentication failed
        }
    } catch {
        // Handle error
        throw error
    }
}
```

In the example above, we define an `authenticateWithBio` function with the `async` keyword. It checks if biometrics authentication is available and throws an error if not. Inside the `do` block, we use `await` to wait for the authentication to complete. The resulting status is then evaluated to determine if the authentication was successful or not.

## Handling Errors

When working with asynchronous code, errors can occur. It's important to handle any potential errors to ensure a smooth user experience. In the example code above, we catch any errors that occur during the authentication process and propagate them using `throw`. You can then handle these errors appropriately, such as displaying an error message to the user.

## Conclusion

Using `async/await` with biometrics authentication in Swift allows us to write more readable and concise code. It simplifies the process of handling biometrics authentication and improves the overall user experience. By checking for biometrics availability and using the `async/await` syntax, we can easily integrate biometrics authentication into our iOS apps in a seamless way.

#biometrics #Swift