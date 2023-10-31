---
layout: post
title: "Building a subscription-based content platform using Swift Vapor"
description: " "
date: 2023-10-31
tags: []
comments: true
share: true
---

In this blog post, we will explore how to build a subscription-based content platform using Swift and Vapor. We will utilize the powerful web application framework Vapor to handle routing, middleware, and authentication, while incorporating subscription management for managing content access. Swift, known for its safety, speed, and simplicity, is an excellent choice for building scalable and robust web applications. So let's dive in and see how we can create a subscription-based content platform using Swift Vapor!

## Table of Contents

- [Setting Up Vapor Project](#setting-up-vapor-project)
- [Implementing User Authentication](#implementing-user-authentication)
- [Configuring Payment Gateway](#configuring-payment-gateway)
- [Managing Subscriptions](#managing-subscriptions)
- [Providing Content Access](#providing-content-access)
- [Conclusion](#conclusion)

## Setting Up Vapor Project

The first step is to set up a new Vapor project. We can use the Vapor Toolbox to create a new project scaffold with a command like:

```swift
vapor new SubscriptionPlatform
```

This will create a new Vapor project named "SubscriptionPlatform." We can then navigate into the project directory and start implementing our subscription-based content platform.

## Implementing User Authentication

To handle user authentication, we need to implement a user model and authentication routes. We can create a User model with properties like email and password using Vapor's `Model` and `Migration` protocols. We can also generate authentication routes using Vapor's `Authenticatable` protocol and Vapor's `UserAuthenticatable` middleware.

```swift
final class User: Model {
    // Properties
    
    static let schema = "users"
    
    @Field(key: "email")
    var email: String
    
    @Field(key: "password")
    var password: String
    
    // Initialization
    
    init() { }
    
    init(email: String, password: String) {
        self.email = email
        self.password = password
    }
}

extension User: Authenticatable { }

extension User: UserAuthenticatable {

    public static var usernameKey: KeyPath<User, Field<String>> {
        return \.$email
    }

    public static var passwordHashKey: KeyPath<User, Field<String>> {
        return \.$password
    }
}
```

With the user model and authentication routes in place, users can register, log in, and log out of the platform.

## Configuring Payment Gateway

Next, we need to integrate a payment gateway to handle subscription payments. One popular choice is Stripe, a widely-used payment processing platform. To configure Stripe in our Vapor project, we can add the `Stripe` package as a dependency in our `Package.swift` file.

```swift
// Package.swift

dependencies: [
    // ...
    .package(url: "https://github.com/vapor-community/stripe-kit.git", from: "1.0.0"),
],
targets: [
    .target(
        // ...
        dependencies: [
            // ...
            .product(name: "StripeKit", package: "stripe-kit"),
        ]
    ),
]
```

With the Stripe package added, we can configure our Stripe API keys and implement the necessary routes for handling subscription payments.

## Managing Subscriptions

To manage subscriptions, we need to create a Subscription model and define the relationship between subscribers (users) and subscriptions. We can create a Subscription model with properties like name, price, and duration. We can also establish a one-to-many relationship between users and subscriptions.

```swift
final class Subscription: Model {
    // Properties
    
    static let schema = "subscriptions"
    
    @Field(key: "name")
    var name: String
    
    @Field(key: "price")
    var price: Double
    
    @Field(key: "duration")
    var duration: TimeInterval
    
    @Parent(key: "user_id")
    var user: User
    
    // Initialization
    
    init() { }
    
    init(name: String, price: Double, duration: TimeInterval) {
        self.name = name
        self.price = price
        self.duration = duration
    }
}
```

With the Subscription model in place and the relationship defined, we can now manage user subscriptions, including creating new subscriptions and associating them with users.

## Providing Content Access

Finally, we need to implement the logic for providing content access based on a user's subscription status. Whenever a user requests access to a protected content endpoint, we can check their subscription status and grant or deny access accordingly.

```swift
func contentHandler(req: Request) throws -> EventLoopFuture<Response> {
    guard let user = req.auth.get(User.self) else {
        throw Abort(.unauthorized)
    }
    
    return User.find(user.id, on: req.db)
        .unwrap(or: Abort(.notFound))
        .flatMap { foundUser in
            return foundUser.$subscription.load(on: req.db)
                .map { subscriptions in
                    guard let subscription = subscriptions.first else {
                        throw Abort(.forbidden)
                    }
                    
                    if subscription.isValid {
                        // Provide content access
                        return req.eventLoop.makeSucceededFuture(Response(status: .ok))
                    } else {
                        throw Abort(.forbidden)
                    }
                }
        }
}
```

By implementing the content access logic, we can restrict access to certain routes based on a user's subscription status.

## Conclusion

In this blog post, we explored how to build a subscription-based content platform using Swift and Vapor. We set up a Vapor project, implemented user authentication, configured a payment gateway, managed subscriptions, and provided content access based on subscription status. By leveraging the power of Swift and Vapor, we can create a robust and scalable subscription-based content platform. Stay tuned for more exciting blog posts on Swift and Vapor!

### References

- [Vapor documentation](https://docs.vapor.codes)
- [Stripe documentation](https://stripe.com/docs)