---
layout: post
title: "Working with async await in voice recognition in Swift"
description: " "
date: 2023-10-04
tags: [selector]
comments: true
share: true
---

Voice recognition has become a popular feature in many applications, allowing users to interact with their devices hands-free. Asynchronous programming is crucial when working with voice recognition, as the process of converting speech into text and performing the necessary actions can take time. In this article, we will explore how to implement async/await in voice recognition in Swift.

## 1. Setting Up the Project

Before we dive into the code, let's set up our project. Open Xcode and create a new Swift project. Choose a suitable name and other project settings.

## 2. Import Speech Framework

To work with voice recognition, we need to import the Speech framework into our project. Follow these steps to import the framework:

1. In the Project Navigator, select your project.
2. Go to the **Build Phases** tab.
3. Under **Link Binary With Libraries**, click on the **+** button.
4. Search for **Speech.framework** and add it to your project.

## 3. Requesting Speech Authorization

First, we need to request authorization from the user to access speech recognition. Add the following code in the `AppDelegate.swift` file:

```swift
import Speech

func requestSpeechRecognitionAuthorization() {
    SFSpeechRecognizer.requestAuthorization { authStatus in
        if authStatus == .authorized {
            print("Speech recognition authorized!")
        } else {
            print("Speech recognition not authorized.")
        }
    }
}

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    requestSpeechRecognitionAuthorization()
    // Other app setup code...
    return true
}
```

This code requests authorization from the user to access speech recognition. If the user grants permission, it prints a success message; otherwise, it prints a failure message.

## 4. Implementing Voice Recognition

Now, let's implement the actual voice recognition functionality. Create a new Swift file called `VoiceRecognition.swift` and add the following code:

```swift
import Speech

typealias RecognitionCompletion = (String) -> Void

class VoiceRecognition: NSObject, SFSpeechRecognizerDelegate {
    private let speechRecognizer = SFSpeechRecognizer(locale: Locale(identifier: "en-US"))
    private var recognitionCompletion: RecognitionCompletion?
    
    func recognizeSpeech(completion: @escaping RecognitionCompletion) async throws {
        recognitionCompletion = completion
        
        guard let recognitionRequest = createRecognitionRequest() else {
            throw RecognitionError.requestError
        }
        
        guard let audioEngine = createAudioEngine() else {
            throw RecognitionError.audioEngineError
        }
        
        audioEngine.prepare()
        
        try await audioEngine.start()
        
        speechRecognizer?.recognitionTask(with: recognitionRequest) { [weak self] (result, error) in
            guard let self = self else { return }
            
            if let result = result {
                let recognizedText = result.bestTranscription.formattedString
                self.recognitionCompletion?(recognizedText)
            } else if let error = error {
                print("Speech recognition failed with error: \(error.localizedDescription)")
                self.recognitionCompletion?("")
            }
            
            audioEngine.stop()
            recognitionRequest.endAudio()
            
            self.recognitionCompletion = nil
        }
    }
    
    private func createRecognitionRequest() -> SFSpeechAudioBufferRecognitionRequest? {
        let recognitionRequest = SFSpeechAudioBufferRecognitionRequest()
        recognitionRequest.shouldReportPartialResults = true
        return recognitionRequest
    }
    
    private func createAudioEngine() -> AVAudioEngine? {
        let audioEngine = AVAudioEngine()
        let inputNode = audioEngine.inputNode
        
        let recordingFormat = inputNode.outputFormat(forBus: 0)
        inputNode.installTap(onBus: 0, bufferSize: 1024, format: recordingFormat) { (buffer, _) in
            self.recognitionRequest?.append(buffer)
        }
        
        return audioEngine
    }
}

enum RecognitionError: Error {
    case requestError
    case audioEngineError
}
```

This class sets up voice recognition by initializing an `SFSpeechRecognizer` and handling the recognition task. The `recognizeSpeech` function is marked as `async` and uses the `await` keyword to wait for the recognition task to complete. The recognized text is passed to the completion handler.

## 5. Using the Voice Recognition

To use the voice recognition functionality, let's create a simple button in our `ViewController.swift` file:

```swift
import UIKit

class ViewController: UIViewController {
    private let voiceRecognition = VoiceRecognition()

    override func viewDidLoad() {
        super.viewDidLoad()
        
        let startButton = UIButton(type: .system)
        startButton.setTitle("Start Recognition", for: .normal)
        startButton.addTarget(self, action: #selector(startRecognition), for: .touchUpInside)
        view.addSubview(startButton)
    }
    
    @objc func startRecognition() async {
        do {
            let recognizedText = try await voiceRecognition.recognizeSpeech()
            print("Recognized text: \(recognizedText)")
        } catch {
            print("Voice recognition failed with error: \(error)")
        }
    }
}
```

In this code, we initialize the `VoiceRecognition` instance and call the `recognizeSpeech` function asynchronously when the button is tapped. The recognized text is printed to the console.

## Conclusion

With async/await, working with voice recognition in Swift becomes more straightforward and cleaner. By making use of the Speech framework and implementing the necessary functions, you can seamlessly integrate voice recognition into your applications. Remember to handle authorization requests and provide proper error handling to ensure a smooth user experience.