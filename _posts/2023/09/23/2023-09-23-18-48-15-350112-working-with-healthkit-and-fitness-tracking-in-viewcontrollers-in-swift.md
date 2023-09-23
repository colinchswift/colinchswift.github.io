---
layout: post
title: "Working with HealthKit and fitness tracking in ViewControllers in Swift"
description: " "
date: 2023-09-23
tags: [healthkit, fitnesstracking]
comments: true
share: true
---

In this blog post, we will explore how to work with HealthKit and implement fitness tracking functionality in ViewControllers using Swift. HealthKit is a powerful framework provided by Apple that allows developers to interact with the user's health data.

Before we begin, make sure you have imported the HealthKit framework in your project and that you have included the necessary permissions in your app's Info.plist file.

## Step 1: Requesting Authorization

The first step is to request the user's authorization to access their health data. We do this by creating an instance of `HKHealthStore` and calling `requestAuthorization(toShare:read:completion:)` method. This method takes two arrays of `HKObjectType` representing the data types we want to write and read from HealthKit. 

```swift
import HealthKit

func requestHealthKitAuthorization() {
    let healthStore = HKHealthStore()
    
    let typesToShare: Set<HKSampleType> = [
        HKObjectType.workoutType()
        // Add other types you want to write here
    ]
    
    let typesToRead: Set<HKObjectType> = [
        HKObjectType.workoutType(),
        // Add other types you want to read here
    ]
    
    healthStore.requestAuthorization(toShare: typesToShare, read: typesToRead) { (success, error) in
        if let error = error {
            print("Error requesting HealthKit authorization: \(error.localizedDescription)")
        } else if success {
            print("HealthKit authorization granted")
        } else {
            print("HealthKit authorization denied")
        }
    }
}
```

## Step 2: Retrieving Health Data

To retrieve health data, we can use `HKSampleQuery` or `HKStatisticsQuery` to perform queries on the store. For example, to retrieve the user's step count, we can use a `HKSampleQuery` as follows:

```swift
func getStepCount(completion: @escaping (Double?, Error?) -> Void) {
    let healthStore = HKHealthStore()
    
    let stepCountType = HKObjectType.quantityType(forIdentifier: .stepCount)!
    
    let sortDescriptor = NSSortDescriptor(key: HKSampleSortIdentifierStartDate, ascending: false)
    let query = HKSampleQuery(sampleType: stepCountType, predicate: nil, limit: 1, sortDescriptors: [sortDescriptor]) { (query, samples, error) in
        if let error = error {
            completion(nil, error)
            return
        }
        
        guard let stepCount = samples?.first as? HKQuantitySample else {
            completion(nil, nil)
            return
        }
        
        completion(stepCount.quantity.doubleValue(for: HKUnit.count()), nil)
    }
    
    healthStore.execute(query)
}
```

## Step 3: Tracking Fitness Data

To track fitness data, we can use `HKWorkoutSession` and `HKLiveWorkoutBuilder`. Here's an example of how to start a workout session and track the user's heart rate:

```swift
import CoreLocation

class WorkoutViewController: UIViewController, CLLocationManagerDelegate {
    
    let healthStore = HKHealthStore()
    let locationManager = CLLocationManager()
    var workoutSession: HKWorkoutSession?
    var workoutBuilder: HKLiveWorkoutBuilder?
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        locationManager.delegate = self
        locationManager.requestWhenInUseAuthorization()
        
        startWorkoutSession()
    }
    
    func startWorkoutSession() {
        let configuration = HKWorkoutConfiguration()
        configuration.activityType = .running
        
        do {
            workoutSession = try HKWorkoutSession(healthStore: healthStore, configuration: configuration)
            
            workoutBuilder = workoutSession?.associatedWorkoutBuilder()
            
            workoutBuilder?.dataSource = HKLiveWorkoutDataSource(healthStore: healthStore, workoutConfiguration: configuration)
            
            workoutBuilder?.delegate = self
            
            workoutSession?.startActivity(with: Date())
            
            // Start tracking location updates
            locationManager.startUpdatingLocation()
        } catch {
            print("Failed to start workout session: \(error.localizedDescription)")
        }
    }
    
    // Location manager delegate methods
    // ...
}

extension WorkoutViewController: HKLiveWorkoutBuilderDelegate {
    func workoutBuilderDidCollectEvent(_ workoutBuilder: HKLiveWorkoutBuilder) {
        guard let heartRateSample = workoutBuilder.statistics(for: HKObjectType.quantityType(forIdentifier: .heartRate)!)?.mostRecentQuantity()?.doubleValue(for: HKUnit.count().unitDivided(by: HKUnit.minute()))) else { return }
        
        // Update UI with heart rate data
    }
}
```

In this example, we create a `HKWorkoutSession` and associate an `HKLiveWorkoutBuilder` with it. We also start updating the user's location through the `CLLocationManager` for route tracking purposes. The `HKLiveWorkoutBuilderDelegate` method is used to collect heart rate data and update the UI accordingly.

## Conclusion

HealthKit provides us with powerful capabilities for fitness tracking and health data management. In this blog post, we learned how to request authorization, retrieve health data, and track fitness data using HealthKit in ViewControllers with Swift. With these concepts, you can now enrich your health and fitness apps with valuable information from HealthKit. Start exploring and building amazing experiences for your users!

#healthkit #fitnesstracking