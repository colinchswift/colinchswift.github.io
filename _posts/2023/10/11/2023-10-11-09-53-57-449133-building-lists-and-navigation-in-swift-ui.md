---
layout: post
title: "Building lists and navigation in Swift UI"
description: " "
date: 2023-10-11
tags: []
comments: true
share: true
---

One of the key features of SwiftUI is the ability to easily create lists and navigation in your app's user interface. Whether you need a simple list of items or a more complex nested structure, SwiftUI makes it a breeze to create and customize navigational flow in your app. In this blog post, we'll explore how to build lists and implement navigation in SwiftUI.

## Creating a List

To start, let's create a simple list of items using the SwiftUI `List` view. The `List` view provides a container for displaying rows of data, and it automatically handles the scrolling and layout for you.

Here's an example of how to create a basic list in SwiftUI:

```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        List {
            Text("Item 1")
            Text("Item 2")
            Text("Item 3")
        }
    }
}
```

In the example above, we've created a `List` view with three rows, each containing a `Text` view. As you can see, SwiftUI's declarative syntax makes it easy to define the content of your list.

## Adding Navigation

Next, let's add navigation to our list so that tapping on a row takes us to a detail view. SwiftUI provides the `NavigationLink` view for this purpose. 

Here's an updated version of our `ContentView` that includes navigation:

```swift
import SwiftUI

struct DetailView: View {
    var body: some View {
        Text("Detail view")
    }
}

struct ContentView: View {
    var body: some View {
        NavigationView {
            List {
                NavigationLink(destination: DetailView()) {
                    Text("Item 1")
                }
                NavigationLink(destination: DetailView()) {
                    Text("Item 2")
                }
                NavigationLink(destination: DetailView()) {
                    Text("Item 3")
                }
            }
            .navigationBarTitle("Items")
        }
    }
}
```

In this example, we've wrapped our `List` view in a `NavigationView` to enable the navigation capabilities. Each `NavigationLink` is associated with a destination view, in this case, the `DetailView`. Tapping on a row will push the `DetailView` onto the navigation stack.

## Passing Data and Customizing Navigation

In SwiftUI, you can easily pass data between views and customize the navigation behavior. To pass data, you simply pass a parameter to the destination view's initializer. To customize the navigation behavior, you can use modifiers on the `NavigationLink` view.

Let's take a look at an updated version of our `DetailView` that receives data from the tapped row:

```swift
import SwiftUI

struct DetailView: View {
    var item: String

    var body: some View {
        Text("Detail view for \(item)")
    }
}
```

And here's how we can pass different data to each `DetailView` instance in our `ContentView`:

```swift
import SwiftUI

struct ContentView: View {
    let items = ["Item 1", "Item 2", "Item 3"]

    var body: some View {
        NavigationView {
            List {
                ForEach(items, id: \.self) { item in
                    NavigationLink(destination: DetailView(item: item)) {
                        Text(item)
                    }
                }
            }
            .navigationBarTitle("Items")
        }
    }
}
```

In this updated example, we've used the `ForEach` view to iterate over our `items` array and create a `NavigationLink` for each item. Each `DetailView` receives the corresponding item as input.

## Conclusion

In this blog post, we've explored how to build lists and implement navigation in SwiftUI. From creating a simple list to adding navigation and passing data between views, SwiftUI makes it easy and intuitive to build dynamic and interactive interfaces. With its declarative syntax, customizable views, and built-in navigation capabilities, SwiftUI is a powerful tool for building modern iOS and macOS apps. Start exploring and experimenting with lists and navigation in your next SwiftUI project!