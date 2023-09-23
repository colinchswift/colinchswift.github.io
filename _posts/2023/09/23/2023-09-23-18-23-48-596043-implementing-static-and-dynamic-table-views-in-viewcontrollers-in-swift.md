---
layout: post
title: "Implementing static and dynamic table views in ViewControllers in Swift"
description: " "
date: 2023-09-23
tags: [Swift, iOSDevelopment]
comments: true
share: true
---

Table views are a common UI component used in iOS app development to display data in a structured and organized manner. In this blog post, we will explore how to implement both static and dynamic table views in ViewControllers using Swift.

## Static Table View

A static table view is one where the layout and the content of the cells are known and fixed at design time. It is useful when you have a small amount of content that doesn't change dynamically. Here's how you can implement a static table view:

1. Open up your storyboard file and drag and drop a Table View Controller onto the canvas.

2. Customize the table view cells by dragging and dropping various UI components onto each cell. You can choose the type of cell you want (such as Basic, Subtitle, etc.) and add labels, images, and other UI controls as needed.

3. Select the table view cell and open the Attributes inspector. Set the Identifier for each cell to a unique value, e.g., "Cell1", "Cell2", etc.

4. Implement the UITableViewDataSource protocol in your TableViewController class. 

```swift
class TableViewController: UITableViewController {
    override func numberOfSections(in tableView: UITableView) -> Int {
        return 1
    }
    
    override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return 2 // Number of rows in your static table view
    }
    
    override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "Cell\(indexPath.row + 1)", for: indexPath)
        // Customize the cell content if needed
        return cell
    }
}
```

5. In your storyboard, select the Table View Controller and open the Connections inspector. Connect the `dataSource` outlet to your TableViewController.

6. Build and run your app to see the static table view populated with the predefined cells.

## Dynamic Table View

Unlike static table views, dynamic table views are used when the number of cells or their content needs to change dynamically. Here's how you can implement a dynamic table view:

1. Follow steps 1 to 3 from the static table view instructions.

2. Implement the UITableViewDataSource protocol in your ViewController class, or any other class that acts as its data source.

```swift
class ViewController: UIViewController, UITableViewDataSource {
    var data = ["Item 1", "Item 2", "Item 3"] // your dynamic data
    
    @IBOutlet weak var tableView: UITableView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        tableView.dataSource = self
    }
    
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return data.count
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath)
        cell.textLabel?.text = data[indexPath.row]
        return cell
    }
}
```

3. In your storyboard, add a Table View component to your ViewController and set its dataSource to the View Controller.

4. Build and run your app to see the dynamic table view populated with the data from the `data` array.

## Conclusion

Implementing static and dynamic table views in ViewControllers using Swift is essential for creating organized and user-friendly interfaces. Whether you need predefined cells or dynamically changing content, table views provide a powerful way to display data in iOS apps. Use the guidelines and code examples provided in this blog post to get started with implementing table views in your own app. #Swift #iOSDevelopment