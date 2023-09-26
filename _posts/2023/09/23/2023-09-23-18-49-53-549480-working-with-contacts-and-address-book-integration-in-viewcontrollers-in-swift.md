---
layout: post
title: "Working with Contacts and Address Book integration in ViewControllers in Swift"
description: " "
date: 2023-09-23
tags: []
comments: true
share: true
---

In an iOS app, it is often necessary to access the device's contacts and address book to retrieve and display contact information or to allow the user to select contacts for certain actions. In this blog post, we will explore how to work with contacts and address book integration within ViewControllers using Swift.

## Importing the Contacts Framework

To begin, we need to import the Contacts framework, which provides APIs for working with contacts and the address book. Add the following import statement at the top of your ViewController file:

```swift
import Contacts
```

## Requesting Authorization

Before accessing the user's contacts, we need to request authorization. Add the following code to your ViewController's `viewDidLoad()` method:

```swift
let store = CNContactStore()
store.requestAccess(for: .contacts) { (granted, error) in
    if granted {
        // Permission granted, proceed with accessing contacts
    } else {
        // Permission not granted, handle the error appropriately
    }
}
```

This code requests the user's authorization to access their contacts. If the user grants permission, you can proceed with accessing the contacts. If not, you should handle the error appropriately.

## Fetching Contacts

To fetch a list of all contacts from the address book, use the `CNContactFetchRequest` class. Add the following code to fetch contacts:

```swift
let keys = [CNContactGivenNameKey, CNContactFamilyNameKey, CNContactEmailAddressesKey]
let request = CNContactFetchRequest(keysToFetch: keys as [CNKeyDescriptor])

do {
    try store.enumerateContacts(with: request) { (contact, stop) in
        // Access contact details here
    }
} catch {
    // Handle error
}
```

In this example, we specify the keys we want to fetch for each contact (such as given name, family name, and email addresses). Then, we create a request with these keys and enumerate over the contacts using the `store.enumerateContacts(with: request)` method.

Inside the closure, you can access the details of each contact using the `contact` parameter.

## Displaying Contacts in UI

To display the fetched contacts in a user interface, you can use standard UIKit components, such as UITableView or UICollectionView, to present the contact information.

## Conclusion

Working with contacts and address book integration in ViewControllers provides a seamless way to access and display user's contacts within your iOS app. By following the steps outlined in this blog post, you can easily request authorization, fetch contacts, and display them in your app's UI.

#iOS #Swift