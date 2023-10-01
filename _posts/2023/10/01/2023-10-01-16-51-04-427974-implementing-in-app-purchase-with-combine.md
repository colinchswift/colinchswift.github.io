---
layout: post
title: "Implementing in-app purchase with Combine"
description: " "
date: 2023-10-01
tags: [iOSDevelopment, InAppPurchases]
comments: true
share: true
---

In-app purchases play a significant role in generating revenue for many iOS applications. With the introduction of the Combine framework, Apple has provided us with a powerful tool to handle asynchronous events and data streams. In this article, we will explore how to implement in-app purchases using Combine in Swift.

## Setting up In-App Purchase

Before diving into the implementation details, let's quickly go through the necessary steps to set up in-app purchases in your iOS app:

1. Enable In-App Purchase capability in your app's project settings.
2. Create an App Store Connect record for your in-app purchase, specifying the product identifier and other details.
3. Generate and download the provisioning profile containing the In-App Purchase entitlements.
4. Add the StoreKit framework to your Xcode project.

## Creating a StoreManager

To handle the in-app purchase process, we need to create a `StoreManager` class that will manage the interactions with the App Store. This class will be responsible for fetching the available products, initiating the purchase, and handling the transaction callbacks. Let's start by creating the `StoreManager` class:

```swift
import Combine
import StoreKit

class StoreManager: NSObject, SKProductsRequestDelegate, SKPaymentTransactionObserver {
    
    private var productsRequest: SKProductsRequest?
    
    private let availableProductsSubject = PassthroughSubject<[SKProduct], Error>()
    private let purchaseCompletedSubject = PassthroughSubject<SKPaymentTransaction, Error>()

    // MARK: - Public API
    
    func fetchProducts(with identifiers: Set<String>) -> Future<[SKProduct], Error> {
        Future { [weak self] promise in
            self?.productsRequest = SKProductsRequest(productIdentifiers: identifiers)
            self?.productsRequest?.delegate = self
            self?.productsRequest?.start()
            
            // Resolve the promise with available products or an error
            self?.availableProductsSubject.sink(receiveCompletion: { completion in
                if case let .failure(error) = completion {
                    promise(.failure(error))
                }
            }, receiveValue: { products in
                promise(.success(products))
            })
            .store(in: &self?.cancellables)
        }
    }
    
    func purchase(product: SKProduct) -> Future<SKPaymentTransaction, Error> {
        Future { [weak self] promise in
            let payment = SKPayment(product: product)
            SKPaymentQueue.default().add(self)
            SKPaymentQueue.default().add(payment)
            
            // Resolve the promise with completed transaction or an error
            self?.purchaseCompletedSubject.sink(receiveCompletion: { completion in
                if case let .failure(error) = completion {
                    promise(.failure(error))
                }
            }, receiveValue: { transaction in
                promise(.success(transaction))
            })
            .store(in: &self?.cancellables)
        }
    }
    
    // MARK: - SKProductsRequestDelegate
    
    func productsRequest(_ request: SKProductsRequest, didReceive response: SKProductsResponse) {
        let products = response.products
        availableProductsSubject.send(products)
        availableProductsSubject.send(completion: .finished)
    }
    
    func request(_ request: SKRequest, didFailWithError error: Error) {
        availableProductsSubject.send(completion: .failure(error))
    }
    
    // MARK: - SKPaymentTransactionObserver
    
    func paymentQueue(_ queue: SKPaymentQueue, updatedTransactions transactions: [SKPaymentTransaction]) {
        for transaction in transactions {
            switch transaction.transactionState {
            case .purchased, .restored:
                SKPaymentQueue.default().finishTransaction(transaction)
                purchaseCompletedSubject.send(transaction)
                purchaseCompletedSubject.send(completion: .finished)
            case .failed:
                SKPaymentQueue.default().finishTransaction(transaction)
                purchaseCompletedSubject.send(completion: .failure(transaction.error ?? StoreError.unknownError))
            default:
                break
            }
        }
    }
    
}
```

## Using Combine to Handle In-App Purchase

Now that we have our `StoreManager` in place, let's see how we can use Combine to handle the in-app purchase flow. Here's an example of how you can fetch available products and initiate a purchase:

```swift
let storeManager = StoreManager()

storeManager.fetchProducts(with: ["com.yourapp.product"]) // Replace with your own product identifier
    .sink(receiveCompletion: { completion in
        if case let .failure(error) = completion {
            // Handle error
        }
    }, receiveValue: { products in
        if let product = products.first {
            storeManager.purchase(product: product)
                .sink(receiveCompletion: { completion in
                    if case let .failure(error) = completion {
                        // Handle error
                    }
                }, receiveValue: { transaction in
                    // Purchase completed successfully
                })
                .store(in: &cancellables)
        }
    })
    .store(in: &cancellables)
```

In the above example, we first fetch available products using the `fetchProducts(with:)` method of the `StoreManager`. Once we have the products, we initiate a purchase using the `purchase(product:)` method. The result of the purchase is then handled through Combine's `sink` operator.

## Conclusion

In this article, we explored how to implement in-app purchases using Combine in Swift. By leveraging the power of Combine, we can easily manage the asynchronous nature of the in-app purchase process and handle transactions in a reactive and declarative way. Combine provides a robust way to handle complex event chains and ensures a smooth purchase experience for users while generating revenue for your iOS app.#iOSDevelopment #InAppPurchases