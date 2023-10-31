---
layout: post
title: "Building a news aggregator and content curation platform using Swift Vapor"
description: " "
date: 2023-10-31
tags: []
comments: true
share: true
---

In today's digital age, staying on top of the latest news and trends can be challenging. With the abundance of information available online, it can be overwhelming to filter out relevant content. That's where news aggregators and content curation platforms come into play.

In this blog post, we will explore how to build a news aggregator and content curation platform using Swift Vapor, a powerful web framework for building web applications and APIs in the Swift programming language.

## Table of Contents
- [Introduction](#introduction)
- [Setting Up the Project](#setting-up-the-project)
- [Fetching News from Various Sources](#fetching-news-from-various-sources)
- [Processing and Storing News Data](#processing-and-storing-news-data)
- [Building the User Interface](#building-the-user-interface)
- [Implementing Content Curation](#implementing-content-curation)
- [Conclusion](#conclusion)

## Introduction

A news aggregator is a platform that collects and displays news articles from various sources into a single location. It allows users to stay updated with the latest news without having to visit multiple websites. Additionally, a content curation platform enables users to curate and share content that they find interesting with others.

## Setting Up the Project

To get started, we'll first need to set up a new Vapor project. Open a terminal and run the following command:

```shell
vapor new NewsAggregator --template=web`
```

This will create a new Vapor project with the basic project structure and dependencies.

## Fetching News from Various Sources

Next, we'll implement the functionality to fetch news articles from various sources. We can leverage APIs provided by news providers such as NewsAPI, RSS feeds, or scrape websites directly using frameworks like SwiftSoup.

We'll use the NewsAPI service for simplicity. Create a new provider to handle the API requests and responses. 

```swift
import Vapor

struct NewsAPIProvider {
    private let baseUrl = "https://newsapi.org/v2/"
    private let apiKey = "YOUR_API_KEY"

    func fetchTopHeadlines() -> EventLoopFuture<[Article]> {
        let url = "\(baseUrl)top-headlines?country=us&apiKey=\(apiKey)"
        return httpClient.get(url) { response in
            guard response.status == .ok else {
                throw Abort(.internalServerError)
            }
            return try response.content.decode(NewsAPIResponse.self).articles
        }
    }
}
```

Make sure to replace "YOUR_API_KEY" with your actual API key from NewsAPI. Here, we are fetching the top headlines from the US, but you can customize the query parameters to suit your needs.

## Processing and Storing News Data

Once we receive the news articles, we need to process and store them in our application's database. We'll use Fluent, the Vapor's database toolkit, to define models and perform database-related operations.

Create a model for the news article:

```swift
final class Article: Model, Content {
    static let schema = "articles"

    @ID(key: .id)
    var id: UUID?

    @Field(key: "title")
    var title: String

    @Field(key: "source")
    var source: String

    @Field(key: "url")
    var url: String

    // Other fields like description, content, publishedAt, etc.

    init() {}

    init(id: UUID? = nil, title: String, source: String, url: String) {
        self.id = id
        self.title = title
        self.source = source
        self.url = url
    }
}
```

In your route handler, fetch the news articles and store them in the database:

```swift
import Vapor

func getTopHeadlines(_ req: Request) throws -> EventLoopFuture<[Article]> {
    let provider = NewsAPIProvider()
    let articles = try provider.fetchTopHeadlines().map { articles in
        let models = articles.map { Article(id: nil, title: $0.title, source: $0.source.name, url: $0.url) }
        return models.save(on: req.db).transform(to: models)
    }
    return articles.flatMap { $0 }
}
```

## Building the User Interface

Now that we have the news articles stored in the database, we can build the user interface to display them. Vapor provides templating support using Leaf, a powerful template engine. You can use Leaf to create dynamic HTML templates and render them with the fetched data.

Create a Leaf template to display the news articles:

```html
<!DOCTYPE html>
<html>
<head>
    <title>News Aggregator</title>
</head>
<body>
    <h1>Top Headlines</h1>
    <ul>
        #for(article in articles) {
            <li>
                <a href="#(article.url)" target="_blank">#(article.title)</a>
                <span>(#(article.source))</span>
            </li>
        }
    </ul>
</body>
</html>
```

Render the template in your route handler and pass the fetched articles:

```swift
import Vapor

func viewTopHeadlines(_ req: Request) throws -> EventLoopFuture<View> {
    let articles = Article.query(on: req.db).all()
    let context: [String: [Article]] = ["articles": articles]
    return req.view.render("topHeadlines", context)
}
```

## Implementing Content Curation

To enable content curation, we can allow users to create accounts, save articles to their profile, and share articles with others. Implement user authentication and authorization using Vapor's `Auth` package. Users can then save and manage their curated content through API endpoints.

## Conclusion

In this blog post, we explored how to build a news aggregator and content curation platform using Swift Vapor. We covered fetching news articles from various sources, processing and storing them in a database, building the user interface using Leaf templates, and implementing content curation functionality.

By leveraging Swift Vapor's powerful features, you can create a robust and scalable platform to help users stay informed and share valuable content with others.

# References
- [Swift Vapor Documentation](https://docs.vapor.codes/)
- [NewsAPI Documentation](https://newsapi.org/docs)
- [SwiftSoup Documentation](https://github.com/scinfu/SwiftSoup)