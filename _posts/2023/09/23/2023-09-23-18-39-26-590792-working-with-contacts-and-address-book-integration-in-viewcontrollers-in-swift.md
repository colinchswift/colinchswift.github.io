---
layout: post
title: "Working with contacts and address book integration in ViewControllers in Swift"
description: " "
date: 2023-09-23
tags: [iOSDevelopment, SwiftProgramming]
comments: true
share: true
---

In this blog post, we will explore how to work with contacts and address book integration in ViewControllers using Swift. We will cover the basics of accessing and displaying contacts, as well as adding new contacts to the address book.

## Accessing Contacts

To access the user's contacts, we need to first request the necessary permissions. We can do this by adding the `NSContactsUsageDescription` key to the app's `Info.plist` file with a brief description of why we need access to contacts. 

To fetch the contacts, we can use the `CNContactStore` class from the Contacts framework. Here is an example of how we can retrieve and display the user's contacts:

```swift
import Contacts

func fetchContacts() {
    let store = CNContactStore()
    let keys = [CNContactGivenNameKey, CNContactFamilyNameKey, CNContactPhoneNumbersKey]
    let request = CNContactFetchRequest(keysToFetch: keys as [CNKeyDescriptor])

    do {
        try store.enumerateContacts(with: request) { (contact, _) in
            let fullName = "\(contact.givenName) \(contact.familyName)"
            var phoneNumbers = ""

            for phoneNumber in contact.phoneNumbers {
                let number = phoneNumber.value.stringValue
                phoneNumbers += "\(number)\n"
            }

            print("Name: \(fullName), Phone Numbers: \(phoneNumbers)")
        }
    } catch {
        print("Error fetching contacts: \(error)")
    }
}
```

This function will fetch the user's contacts and loop through each contact, printing their name and phone numbers.

## Adding Contacts

To add a new contact to the user's address book, we need to create a new `CNMutableContact` object and set its properties accordingly. Here is an example of how we can add a contact:

```swift
func addContact() {
    let store = CNContactStore()
    let contact = CNMutableContact()

    contact.givenName = "John"
    contact.familyName = "Doe"

    let homePhone = CNPhoneNumber(stringValue: "1234567890")
    let homePhoneValue = CNLabeledValue(label: CNLabelHome, value: homePhone)
    contact.phoneNumbers = [homePhoneValue]

    let saveRequest = CNSaveRequest()
    saveRequest.add(contact, toContainerWithIdentifier: nil)

    do {
        try store.execute(saveRequest)
        print("Contact added successfully.")
    } catch {
        print("Error adding contact: \(error)")
    }
}
```

In this example, we create a new `CNMutableContact` object, set its properties like name and phone number, and add it to the address book using a `CNSaveRequest`.

## Conclusion

Working with contacts and address book integration in ViewControllers is essential when building apps that require access to the user's contacts. By using the Contacts framework in Swift, we can easily fetch and display contacts, as well as add new contacts to the address book.

Remember to handle the necessary permissions and provide a clear explanation in the app's `Info.plist` file for accessing contacts. This will ensure a smooth user experience and compliance with privacy guidelines.

#iOSDevelopment #SwiftProgramming