---
layout: post
title: "Handwritten digit recognition with Swift for TensorFlow"
description: " "
date: 2023-09-29
tags: [MachineLearning, Swift]
comments: true
share: true
---

![Swift for TensorFlow](https://www.tensorflow.org/swift/images/swift-tensorflow-logo.png)

**Swift for TensorFlow** is a powerful tool for building machine learning models using the Swift programming language. In this blog post, we will explore how to implement a handwritten digit recognition model using Swift for TensorFlow.

## Dataset

To train our model, we will use the popular MNIST dataset, which consists of 60,000 handwritten digit images for training and 10,000 images for testing. Each image is a grayscale, 28x28 pixel image representing a single digit from 0 to 9.

## Model Architecture

For our digit recognition model, we will use a simple convolutional neural network (CNN) architecture. This CNN will consist of a combination of convolutional layers, pooling layers, and fully connected layers to classify the input digit images.

Here is an example code snippet showing the architecture of our model:

```swift
import TensorFlow

struct DigitClassifier: Layer {
  var conv1 = Conv2D<Float>(filterShape: (3, 3, 1, 32), activation: relu)
  var pool1 = MaxPool2D<Float>(poolSize: (2, 2), strides: (2, 2))
  
  var conv2 = Conv2D<Float>(filterShape: (3, 3, 32, 64), activation: relu)
  var pool2 = MaxPool2D<Float>(poolSize: (2, 2), strides: (2, 2))
  
  var flatten = Flatten<Float>()
  
  var dense1 = Dense<Float>(inputSize: 7 * 7 * 64, outputSize: 128, activation: relu)
  var dense2 = Dense<Float>(inputSize: 128, outputSize: 10)

  @differentiable
  func call(_ input: Tensor<Float>) -> Tensor<Float> {
    var convolved = input.sequenced(through: conv1, pool1, conv2, pool2)
    return convolved.sequenced(through: flatten, dense1, dense2)
  }
}

let model = DigitClassifier()
```

## Training and Evaluation

After defining the model architecture, we can train and evaluate the model using the MNIST dataset. We will split the dataset into training and testing sets and feed the images and their corresponding labels to the model for training.

Here is an example code snippet showing how to train and evaluate the model:

```swift
let (trainImages, trainLabels) = loadMNIST(training: true)
let (testImages, testLabels) = loadMNIST(training: false)

let optimizer = RMSProp(for: model)
let batchSize = 32
let epochCount = 5

for epoch in 0..<epochCount {
  var epochLoss: Float = 0
  var correctGuessCount: Int = 0
  
  for i in 0..<trainImages.shape[0] / batchSize {
    let start = i * batchSize
    let end = start + batchSize
    let images = trainImages.slice(lowerBounds: [start, 0, 0, 0], upperBounds: [end, -1, -1, -1])
    let labels = trainLabels[start..<end]
    
    let (loss, grads) = valueWithGradient(at: model) { model -> Tensor<Float> in
      let logits = model(images / 255)
      return softmaxCrossEntropy(logits: logits, labels: labels)
    }
    optimizer.update(&model.allDifferentiableVariables, along: grads)
    
    epochLoss += loss.scalarized()
    correctGuessCount += Int(Tensor<Int32>(logits.argmax(squeezingAxis: 1)) .== labels).sum().scalarized()
  }
  
  let accuracy = Float(correctGuessCount) / Float(trainImages.shape[0])
  print("Epoch \(epoch): Loss: \(epochLoss), Accuracy: \(accuracy)")
}

let testLogits = model(testImages / 255)
let testLoss = softmaxCrossEntropy(logits: testLogits, labels: testLabels)
let testAccuracy = Float(Tensor<Int32>(testLogits.argmax(squeezingAxis: 1)) .== testLabels).mean().scalarized()
print("Test Loss: \(testLoss.scalarized()), Test Accuracy: \(testAccuracy)")
```

## Conclusion

Swift for TensorFlow provides a convenient and efficient way to build machine learning models in Swift. In this blog post, we have explored how to implement a handwritten digit recognition model using Swift for TensorFlow and the MNIST dataset. With the provided code snippets, you can start experimenting with your own machine learning models and datasets using Swift for TensorFlow.

#MachineLearning #Swift #TensorFlow