---
layout: post
title: "Validating input data with Codable using regular expressions and custom logic"
description: " "
date: 2023-09-22
tags: [Swift, Validation]
comments: true
share: true
---

In any application that deals with user input, it is crucial to ensure that the data being entered is valid and meets the required format. One way to accomplish this is by using regular expressions and custom logic to validate the input data.

With the introduction of the Codable protocol in Swift, validating input data has become even easier. Codable allows you to easily encode and decode types to and from external representations, such as JSON. This gives us a convenient way to validate the data before decoding it.

## Regular Expressions for Validation

Regular expressions are a powerful tool for pattern matching. They provide a concise and flexible way to validate strings against specific patterns.

Let's say we have a user registration form that requires the user to enter a valid email address. We can use a regular expression to enforce this validation. Here's an example of how we can define a regular expression pattern for validating email addresses:

```swift
struct User: Codable {
    let email: String
    
    enum CodingKeys: String, CodingKey {
        case email
    }
    
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        let rawEmail = try container.decode(String.self, forKey: .email)
        
        // Validate the email address using a regular expression
        let emailRegex = #"\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}\b"#
        let emailPredicate = NSPredicate(format:"SELF MATCHES %@", emailRegex)
        
        guard emailPredicate.evaluate(with: rawEmail) else {
            throw DecodingError.dataCorruptedError(forKey: .email, in: container, debugDescription: "Invalid email format")
        }
        
        email = rawEmail
    }
    
    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(email, forKey: .email)
    }
}
```

In this example, we have a User struct that conforms to the Codable protocol. We provide custom implementations for the init(from:) and encode(to:) methods to perform the input validation.

Inside the init(from:) method, we decode the raw email string from the container and then validate it using the regular expression. If the email is invalid, we throw a DecodingError with an appropriate debug description.

## Custom Logic for Validation

In addition to regular expressions, you can also use custom logic to validate input data. For example, let's say we have a password field that needs to meet certain requirements, such as a minimum length and a combination of uppercase and lowercase letters.

We can add custom validation logic inside the init(from:) method to enforce these requirements. Here's an example:

```swift
struct User: Codable {
    let password: String
    
    enum CodingKeys: String, CodingKey {
        case password
    }
    
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        password = try container.decode(String.self, forKey: .password)
        
        // Perform custom validation logic
        guard password.count >= 8 else {
            throw DecodingError.dataCorruptedError(forKey: .password, in: container, debugDescription: "Password should be at least 8 characters long")
        }
        
        let hasUpperCase = password.rangeOfCharacter(from: .uppercaseLetters) != nil
        let hasLowerCase = password.rangeOfCharacter(from: .lowercaseLetters) != nil
        guard hasUpperCase && hasLowerCase else {
            throw DecodingError.dataCorruptedError(forKey: .password, in: container, debugDescription: "Password should contain both uppercase and lowercase letters")
        }
    }
    
    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(password, forKey: .password)
    }
}
```

In this example, we again have a User struct with a password field. Inside the init(from:) method, we perform custom validation logic to ensure that the password meets the requirements. If any of the requirements are not met, we throw a DecodingError with an appropriate debug description.

## Conclusion

Validating input data is an essential part of any application to ensure data integrity and security. By leveraging the power of Codable, regular expressions, and custom logic, we can easily validate input data before decoding it.

Using regular expressions, we can enforce patterns and formats for input fields like email addresses. Additionally, with custom logic, we can enforce requirements such as minimum length and character combinations for passwords.

By combining these techniques, we can build robust and secure applications that handle user input data effectively.

#Swift #Validation