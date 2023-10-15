---
layout: post
title: "Implementing background fingerprint authentication in Swift"
description: " "
date: 2023-10-16
tags: []
comments: true
share: true
---

In this tutorial, we will learn how to implement background fingerprint authentication in Swift. With background fingerprint authentication, users can seamlessly authenticate themselves in your app without having to manually enter a password or use TouchID.

**Table of Contents**
- [Requirements](#requirements)
- [Adding the necessary permissions](#adding-the-necessary-permissions)
- [Implementing the fingerprint authentication](#implementing-the-fingerprint-authentication)
- [Testing the background fingerprint authentication](#testing-the-background-fingerprint-authentication)
- [Conclusion](#conclusion)

## Requirements

To follow along with this tutorial, you will need:
- Xcode 11 or later
- An iOS device with TouchID or FaceID capability
- Basic knowledge of Swift programming language

## Adding the necessary permissions

Before we can implement background fingerprint authentication, we need to add the necessary permissions to our app's `Info.plist` file. Open the `Info.plist` file and add the following key-value pairs:

```swift
<key>NSFaceIDUsageDescription</key>
<string>We use FaceID to securely authenticate you.</string>
```

Make sure to replace the string value with a description of how your app will use FaceID or TouchID.

## Implementing the fingerprint authentication

First, import the `LocalAuthentication` framework into your Swift file:

```swift
import LocalAuthentication
```

Next, we need to check if the device supports biometric authentication. You can use the following code snippet to do that:

```swift
let context = LAContext()
var error: NSError?

if context.canEvaluatePolicy(.deviceOwnerAuthenticationWithBiometrics, error: &error) {
    // Biometric authentication is available
} else {
    // Biometric authentication is not available
}
```

Once you have confirmed that biometric authentication is available, you can proceed with the actual authentication:

```swift
context.evaluatePolicy(.deviceOwnerAuthenticationWithBiometrics, localizedReason: "Authenticate to continue") { (success, error) in
    DispatchQueue.main.async {
        if success {
            // Authentication successful
        } else {
            // Authentication failed
        }
    }
}
```

In the code above, the localizedReason parameter is used to display a message to the user explaining why the authentication is required.

## Testing the background fingerprint authentication

To test the background fingerprint authentication, build and run your app on a device with TouchID or FaceID capability. The device will prompt you to authenticate using your fingerprint or face. If the authentication is successful, you will see the message "Authentication successful" in the code above. Otherwise, you will see the message "Authentication failed".

## Conclusion

In this tutorial, we have learned how to implement background fingerprint authentication in Swift using the LocalAuthentication framework. This feature provides a seamless and secure way for users to authenticate themselves in your app.