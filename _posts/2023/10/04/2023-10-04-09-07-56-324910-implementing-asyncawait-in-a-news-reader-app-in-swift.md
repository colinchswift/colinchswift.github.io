---
layout: post
title: "Implementing async/await in a news reader app in Swift"
description: " "
date: 2023-10-04
tags: [async, await]
comments: true
share: true
---

With the introduction of Swift 5.5, a new concurrency model has been added to the language, making it easier to write asynchronous code. One of the key features of this model is the `async/await` syntax, which allows you to write asynchronous code in a more sequential and readable way.

In this article, we will explore how you can use `async/await` to implement asynchronous operations in a news reader app in Swift.

## Setting up the news reader app

Before diving into the `async/await` syntax, let's first set up the basic structure of our news reader app. We'll assume that you already have a basic understanding of SwiftUI and Combine.

1. Create a new SwiftUI project in Xcode and name it "NewsReaderApp".
2. Open the ContentView.swift file and replace the existing code with the following:

```swift
import SwiftUI

struct ContentView: View {
    @StateObject private var newsViewModel = NewsViewModel()
    
    var body: some View {
        NavigationView {
            List(newsViewModel.newsArticles) { article in
                Text(article.title)
            }
            .navigationTitle("News Reader")
            .onAppear {
                newsViewModel.fetchNewsArticles()
            }
        }
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

3. Next, create a new Swift file called NewsViewModel.swift and add the following code:

```swift
import Foundation

class NewsViewModel: ObservableObject {
    @Published var newsArticles: [NewsArticle] = []

    func fetchNewsArticles() {
        // Fetch news articles asynchronously
    }
}
```

4. Finally, create a new Swift file called NewsArticle.swift and add the following code:

```swift
import Foundation

struct NewsArticle: Identifiable {
    let id = UUID()
    let title: String
}
```

At this point, we have set up the basic structure of our news reader app, with a view to display a list of news articles and a view model to manage the data.

## Implementing async/await with URLSession

To fetch the news articles asynchronously, we can use the `async/await` syntax in combination with URLSession. The `async` keyword indicates that the function is asynchronous, and the `await` keyword is used to wait for the asynchronous operation to complete.

1. In the NewsViewModel.swift file, update the `fetchNewsArticles()` function as follows:

```swift
func fetchNewsArticles() async {
    do {
        let url = URL(string: "https://api.example.com/news")!
        let (data, _) = try await URLSession.shared.data(from: url)
        let decodedResponse = try JSONDecoder().decode([NewsArticle].self, from: data)
        DispatchQueue.main.async {
            self.newsArticles = decodedResponse
        }
    } catch {
        print("Error fetching news articles: \(error)")
    }
}
```

In the above code, we use `URLSession.shared.data(from: url)` with `await` to fetch the news articles asynchronously. After receiving the data, we decode it into an array of `NewsArticle` objects using JSONDecoder.

2. Run the app and you should see the list of news articles populated asynchronously.

## Conclusion

With the new `async/await` syntax in Swift 5.5, handling asynchronous operations has become much easier and more readable. In this article, we explored how to implement `async/await` in a news reader app to fetch and display news articles asynchronously. The `async/await` syntax simplifies the code and makes it more manageable, allowing developers to write more efficient and responsive apps.

Asynchronous programming is an important concept to grasp in modern app development, and Swift's `async/await` syntax provides a powerful and elegant way to handle it.

#swift #async #await