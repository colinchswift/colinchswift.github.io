---
layout: post
title: "Implementing Apple Pay and in-app purchases in ViewControllers in Swift"
description: " "
date: 2023-09-23
tags: [ApplePay, InAppPurchases]
comments: true
share: true
---

#### #ApplePay #InAppPurchases

Apple Pay and in-app purchases are essential features in any iOS app that requires transactions. In this tutorial, we will explore how to implement Apple Pay and in-app purchases in ViewControllers using Swift.

## Setting Up Apple Pay

Before implementing Apple Pay, make sure your app is enrolled in the Apple Developer Program and has a valid merchant ID associated with it.

### Step 1: Import the necessary frameworks

To enable Apple Pay in your app, you need to import the `PassKit` framework. Open your ViewController file and import the framework by adding the following line at the top:

```swift
import PassKit
```

### Step 2: Create a PKPaymentAuthorizationViewController

To present Apple Pay, you need to create a `PKPaymentAuthorizationViewController` and present it. Add the following code to your ViewController class:

```swift
class YourViewController: UIViewController {
    
    // ...
    
    func presentApplePay() {
        if PKPaymentAuthorizationViewController.canMakePayments() {
            let request = PKPaymentRequest()
            // Configure the payment request
            // ...
            
            let paymentController = PKPaymentAuthorizationViewController(paymentRequest: request)
            paymentController.delegate = self
            present(paymentController, animated: true, completion: nil)
        } else {
            // Show an alternative payment option
        }
    }
    
    // ...
    
}
```

### Step 3: Handle the payment authorization delegate methods

To handle the result of the payment, conform to the `PKPaymentAuthorizationViewControllerDelegate` protocol and implement the delegate methods. Place the following code inside your ViewController class:

```swift
extension YourViewController: PKPaymentAuthorizationViewControllerDelegate {
    
    // Handle the payment authorization result
    func paymentAuthorizationViewController(_ controller: PKPaymentAuthorizationViewController, didAuthorizePayment payment: PKPayment, handler completion: @escaping (PKPaymentAuthorizationResult) -> Void) {
        // Process the payment
        
        let paymentResult = PKPaymentAuthorizationResult(status: .success, errors: nil)
        completion(paymentResult)
    }
    
    // Handle the payment authorization completion
    func paymentAuthorizationViewControllerDidFinish(_ controller: PKPaymentAuthorizationViewController) {
        dismiss(animated: true, completion: nil)
    }
    
}
```

## Implementing In-App Purchases

In-app purchases allow you to offer additional content or features within your app. Here is how you can set up in-app purchases in ViewControllers.

### Step 1: Enable in-app purchases in your project

To enable in-app purchases, you need to enable the In-App Purchases capability in your Xcode project. Go to your project settings, select the target, and enable the capability.

### Step 2: Add the necessary code for in-app purchases

Add the following code to your ViewController class to handle in-app purchases:

```swift
class YourViewController: UIViewController {
    
    // ...
    
    func buyProduct() {
        guard let product = SKProductStore.getProduct() else {
            // Product not available or error retrieving products
            return
        }
        
        let payment = SKPayment(product: product)
        SKPaymentQueue.default().add(payment)
    }
    
    // ...
    
}
```

### Step 3: Implement the StoreKit delegate methods

To handle the purchase and verification of in-app products, implement the necessary delegate methods from the `SKPaymentTransactionObserver`. Add the following code to your ViewController class:

```swift
class YourViewController: UIViewController, SKPaymentTransactionObserver {
    
    // ...
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        SKPaymentQueue.default().add(self)
    }
    
    // Handle the purchase and transaction status
    func paymentQueue(_ queue: SKPaymentQueue, updatedTransactions transactions: [SKPaymentTransaction]) {
        for transaction in transactions {
            switch transaction.transactionState {
            case .purchased:
                // Process the purchase
                
                SKPaymentQueue.default().finishTransaction(transaction)
                
            case .restored:
                // Process the restoration
                
                SKPaymentQueue.default().finishTransaction(transaction)
                
            case .failed:
                // Failed transaction
                
                SKPaymentQueue.default().finishTransaction(transaction)
                
            default:
                break
            }
        }
    }
    
    // ...
    
}
```

That's it! Now you have implemented Apple Pay and in-app purchases in your ViewControllers using Swift. Remember to handle errors, user authentication, and product validation appropriately to ensure a smooth and secure payment experience.

Keep in mind that Apple Pay and in-app purchases involve security and financial transactions, so it's crucial to adhere to best practices and follow Apple's guidelines when implementing these features in your app.