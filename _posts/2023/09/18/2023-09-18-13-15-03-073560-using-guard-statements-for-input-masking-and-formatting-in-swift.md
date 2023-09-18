---
layout: post
title: "Using guard statements for input masking and formatting in Swift"
description: " "
date: 2023-09-18
tags: [Swift, InputFormatting]
comments: true
share: true
---

![Swift](https://img.shields.io/badge/Language-Swift-orange)

The way users input and format data is crucial in any application. In Swift, guard statements provide a concise and elegant way to validate and format user input while maintaining code readability. In this blog post, we will explore how to use guard statements for input masking and formatting in Swift.

## Input Masking
Masking user input involves enforcing a specific format for textual data, such as phone numbers, dates, or credit card numbers. By using guard statements, we can easily ensure that the user's input matches the desired format.

### Example: Phone Number Formatting
Let's say we want the user to enter a phone number in the format "(XXX) XXX-XXXX". We can use a guard statement to enforce this format and provide feedback to the user if the input is invalid.

```swift
func formatPhoneNumber(_ phoneNumber: String) -> String? {
    guard phoneNumber.count == 10 else {
        print("Invalid phone number length")
        return nil
    }
    
    let areaCode = phoneNumber.prefix(3)
    let middleDigits = phoneNumber[phoneNumber.index(phoneNumber.startIndex, offsetBy: 3)..<phoneNumber.index(phoneNumber.startIndex, offsetBy: 6)]
    let lastDigits = phoneNumber.suffix(4)
    
    return "(\(areaCode)) \(middleDigits)-\(lastDigits)"
}

// Usage
guard let formattedPhoneNumber = formatPhoneNumber("1234567890") else {
    // Handle invalid phone number
    return
}

print(formattedPhoneNumber) // Output: (123) 456-7890
```
In the example above, we use `guard phoneNumber.count == 10` to ensure that the entered phone number has the correct length. If the length is invalid, we print an error message and return `nil`. Otherwise, we extract the area code, middle digits, and last digits of the phone number and return the formatted string.

## Input Formatting
Apart from enforcing a specific format, guard statements can also be used to convert raw user input into the desired format. This is especially useful when dealing with numerical or date inputs.

### Example: Date Formatting
Consider a scenario where a user needs to input a date in the format "MM/DD/YYYY". We can use a guard statement to validate the input and convert it into a `Date` object.

```swift
func formatDate(_ dateString: String) -> Date? {
    guard dateString.count == 10 else {
        print("Invalid date length")
        return nil
    }
    
    let dateFormatter = DateFormatter()
    dateFormatter.dateFormat = "MM/dd/yyyy"
    
    guard let date = dateFormatter.date(from: dateString) else {
        print("Invalid date format")
        return nil
    }
    
    return date
}

// Usage
guard let formattedDate = formatDate("12/31/2022") else {
    // Handle invalid date
    return
}

print(formattedDate) // Output: 2022-12-31 00:00:00 +0000
```

In the above example, we use a guard statement to check if the input date has the correct length. We then set the desired date format using a `DateFormatter` and convert the input string into a `Date` object. If the conversion fails, we print an error message and return `nil`. Otherwise, we return the formatted date.

## Conclusion
Using guard statements for input masking and formatting in Swift allows us to easily validate and format user input while maintaining code readability. By enforcing specific formats and providing feedback to the user, we can ensure that the data entered into our applications is accurate and consistent.

With guard statements, handling input masking and formatting becomes more manageable and reduces the likelihood of unexpected errors while processing user data. So, make sure to leverage this powerful feature in Swift to improve the user experience in your applications.

---

\#Swift #InputFormatting #GuardStatements