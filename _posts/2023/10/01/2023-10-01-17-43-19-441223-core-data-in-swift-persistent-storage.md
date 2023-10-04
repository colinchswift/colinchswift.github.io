---
layout: post
title: "Core Data in Swift: persistent storage"
description: " "
date: 2023-10-01
tags: [CoreData]
comments: true
share: true
---

Core Data is a powerful framework provided by Apple that allows developers to store and manage data in their Swift applications. It provides a simple way to work with a database, by abstracting away many of the complexities involved. In this blog post, we will focus on using Core Data for persistent storage.

## Setting up Core Data

To start using Core Data in your Swift project, you need to first set up a Core Data stack. Here are the steps to do that:

1. **Create a Data Model**: In Xcode, go to File -> New -> File, and choose "Data Model" under the Core Data section. This will create a `.xcdatamodeld` file where you can define your data model.

2. **Define Entities and Attributes**: In the Data Model editor, you can define entities (representing your data objects) and attributes (representing the properties of your entities). **Entity** can be understood as a database table, and **attribute** as a column.

3. **Generate Managed Object Subclass**: In the Xcode menu, go to Editor -> Create NSManagedObject Subclass. This will generate Swift classes for your entities, which are subclasses of NSManagedObject. These classes will be used to interact with your Core Data objects.

4. **Create a Core Data Stack**: In your AppDelegate.swift file, create a lazy property to create the Core Data stack. The stack includes a managed object model, a persistent store coordinator, and a managed object context. You can use the code snippet below as a starting point:

```swift
lazy var persistentContainer: NSPersistentContainer = {
    let container = NSPersistentContainer(name: "YourDataModel")
    container.loadPersistentStores(completionHandler: { (storeDescription, error) in
        if let error = error as NSError? {
            fatalError("Unresolved error \(error), \(error.userInfo)")
        }
    })
    return container
}()

lazy var managedObjectContext: NSManagedObjectContext = {
    return persistentContainer.viewContext
}()
```

## Saving and Fetching Data

Once you have set up your Core Data stack, you can start saving and fetching data.

To **save** an object, follow these steps:

1. Create a new instance of your entity class using the `NSEntityDescription.insertNewObject(forEntityName:in:)` method.

2. Set the attribute values of the newly created object.

3. Call the `save()` method on the managed object context to persist the changes.

```swift
let entity = NSEntityDescription.entity(forEntityName: "YourEntityName", in: managedObjectContext)!
let object = YourEntityName(entity: entity, insertInto: managedObjectContext)

// Set attribute values
object.name = "John Doe"
object.age = 25

// Save changes
do {
    try managedObjectContext.save()
} catch {
    // Handle error
}
```

To **fetch** data from Core Data, you can use the `NSFetchRequest` class. Here's an example of fetching all objects of a specific entity:

```swift
let fetchRequest: NSFetchRequest<YourEntityName> = YourEntityName.fetchRequest()
do {
    let objects = try managedObjectContext.fetch(fetchRequest)
    for object in objects {
        // Access object attributes
        let name = object.name
        let age = object.age
        
        // Do something with the fetched data
    }
} catch {
    // Handle error
}
```

## Conclusion

Core Data provides a convenient way to store and manage data in your Swift applications. By following the steps outlined in this blog post, you can set up Core Data for persistent storage and start saving and fetching data effortlessly. #Swift #CoreData