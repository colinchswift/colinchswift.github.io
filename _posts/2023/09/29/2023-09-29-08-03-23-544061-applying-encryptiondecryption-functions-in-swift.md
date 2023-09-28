---
layout: post
title: "Applying Encryption/Decryption Functions in Swift"
description: " "
date: 2023-09-29
tags: [encryption, decryption]
comments: true
share: true
---

Encryption and decryption are essential techniques used in secure communication and data storage. In this blog post, we will explore how to implement encryption and decryption functions in Swift, a popular programming language used for iOS and macOS app development.

## 1. Choosing an Encryption Algorithm

Before we dive into the implementation, it's crucial to choose a suitable encryption algorithm. Some commonly used encryption algorithms include:

- **AES (Advanced Encryption Standard)**: A symmetric encryption algorithm widely adopted by industries and governments.
- **RSA (Rivest-Shamir-Adleman)**: An asymmetric encryption algorithm that uses a pair of keys, public and private, for encryption and decryption.

The choice of algorithm depends on the specific requirements and use case of your application. For this blog post, we will focus on implementing the AES encryption and decryption functions in Swift.

## 2. Using CommonCrypto for AES Encryption

Swift provides the `CommonCrypto` framework, which is a wrapper around the underlying C library for cryptographic functions. To use AES encryption, we need to import the `CommonCrypto` module into our Swift code:

```swift
import CommonCrypto
```

Next, let's define a function to encrypt a given plaintext using AES:

```swift
func encryptAES(plaintext: Data, key: Data, iv: Data) -> Data? {
    let keyLength = kCCKeySizeAES256
    let bufferSize = plaintext.count + kCCBlockSizeAES128
    var buffer = Data(count: bufferSize)

    let status = buffer.withUnsafeMutableBytes { bufferPointer in
        plaintext.withUnsafeBytes { dataPointer in
            iv.withUnsafeBytes { ivPointer in
                key.withUnsafeBytes { keyPointer in
                    CCCrypt(CCOperation(kCCEncrypt),
                            CCAlgorithm(kCCAlgorithmAES),
                            CCOptions(kCCOptionPKCS7Padding),
                            keyPointer.baseAddress,
                            keyLength,
                            ivPointer.baseAddress,
                            dataPointer.baseAddress,
                            dataPointer.count,
                            bufferPointer.baseAddress,
                            bufferPointer.count,
                            nil)
                }
            }
        }
    }
    
    if status == kCCSuccess {
        buffer.count = CCCryptorGetOutputLength(keyLength, true, bufferSize)
        return buffer
    } else {
        return nil
    }
}
```

In this function, we use the `CCCrypt` function from `CommonCrypto` to perform the encryption. The `plaintext`, `key`, and `iv` (initialization vector) are required parameters.

## 3. Using CommonCrypto for AES Decryption

Similarly, we can implement a function for AES decryption using `CommonCrypto`:

```swift
func decryptAES(ciphertext: Data, key: Data, iv: Data) -> Data? {
    let keyLength = kCCKeySizeAES256
    let bufferSize = ciphertext.count + kCCBlockSizeAES128
    var buffer = Data(count: bufferSize)
    
    let status = buffer.withUnsafeMutableBytes { bufferPointer in
        ciphertext.withUnsafeBytes { dataPointer in
            iv.withUnsafeBytes { ivPointer in
                key.withUnsafeBytes { keyPointer in
                    CCCrypt(CCOperation(kCCDecrypt),
                            CCAlgorithm(kCCAlgorithmAES),
                            CCOptions(kCCOptionPKCS7Padding),
                            keyPointer.baseAddress,
                            keyLength,
                            ivPointer.baseAddress,
                            dataPointer.baseAddress,
                            dataPointer.count,
                            bufferPointer.baseAddress,
                            bufferPointer.count,
                            nil)
                }
            }
        }
    }
    
    if status == kCCSuccess {
        buffer.count = CCCryptorGetOutputLength(keyLength, true, bufferSize)
        return buffer
    } else {
        return nil
    }
}
```

Here, we use the same `CCCrypt` function with the `CCOperation(kCCDecrypt)` parameter to decrypt the given `ciphertext`.

## 4. Example Usage

Now that we have implemented the encryption and decryption functions, let's see how we can use them:

```swift
let plaintext = "Hello, World!".data(using: .utf8)!
let key = "MySecretKey123456".data(using: .utf8)!
let iv = "InitializationVec".data(using: .utf8)!

if let ciphertext = encryptAES(plaintext: plaintext, key: key, iv: iv) {
    print("Ciphertext: \(ciphertext.base64EncodedString())")
    
    if let decrypted = decryptAES(ciphertext: ciphertext, key: key, iv: iv) {
        let decryptedString = String(data: decrypted, encoding: .utf8)!
        print("Decrypted: \(decryptedString)")
    }
}
```

In this example, we encrypt the plaintext "Hello, World!" using AES with a secret key and an initialization vector. Then, we print the ciphertext and decrypt it back to obtain the original plaintext using the same key and IV.

## Conclusion

In this blog post, we explored how to implement encryption and decryption functions in Swift using the `CommonCrypto` framework. Encryption is crucial for securing sensitive data, and Swift provides powerful tools to achieve that. By understanding these concepts and applying them in your iOS or macOS applications, you can ensure the confidentiality and integrity of your data.

#encryption #decryption