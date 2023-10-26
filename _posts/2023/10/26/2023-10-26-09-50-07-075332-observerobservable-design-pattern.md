---
layout: post
title: "Observer/Observable design pattern"
description: " "
date: 2023-10-26
tags: [ObserverPattern, ObservablePattern]
comments: true
share: true
---

The Observer/Observable design pattern is a behavioral design pattern that allows an object, called the *Observable*, to notify its dependent objects, called *Observers*, about any changes in its state. This pattern provides a loose coupling between objects, enabling easy modification and extension of the system.

## How it works

The Observable maintains a list of Observers and provides methods to register or unregister them. When the Observable's state changes, it notifies all registered Observers by calling a specific method, usually `update()`, on each Observer.

Observers, upon receiving the notification, can then retrieve the updated state from the Observable and perform any necessary actions.

## Benefits

### Loose Coupling

The Observer/Observable design pattern promotes loose coupling between objects. Observers do not need to be aware of the concrete implementation of the Observable, allowing for easy modification or introduction of new Observables without affecting the existing Observers.

### Flexibility and Extensibility

The pattern allows for easy extension and modification of the system. New Observers can be added without modifying the code of the Observable or existing Observers. Similarly, new types of Observables can be introduced without impacting the existing code.

### One-to-Many Relationship

The Observer/Observable pattern is ideal when there is a one-to-many relationship between objects. Multiple Observers can be registered to a single Observable, and they will be notified independently of each other whenever the Observable's state changes.

## Example

Let's consider a simple example of a weather station. The weather station acts as the Observable, while various display devices act as Observers. Whenever the weather station detects a change in weather conditions, it notifies the display devices about the new data.

```java
// Observer interface
public interface Observer {
    void update();
}

// Observable class
public class WeatherStation {
    private List<Observer> observers = new ArrayList<>();
    private WeatherData weatherData;

    public void registerObserver(Observer observer) {
        observers.add(observer);
    }

    public void unregisterObserver(Observer observer) {
        observers.remove(observer);
    }

    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update();
        }
    }

    public void setWeatherData(WeatherData weatherData) {
        this.weatherData = weatherData;
        notifyObservers();
    }

    public WeatherData getWeatherData() {
        return weatherData;
    }
}

// Concrete Observer
public class DisplayDevice implements Observer {
    private WeatherData weatherData;

    @Override
    public void update() {
        weatherData = WeatherStation.getWeatherData();
        display();
    }

    public void display() {
        System.out.println("Current weather: " + weatherData);
    }
}

// Usage
public static void main(String[] args) {
    WeatherStation weatherStation = new WeatherStation();
    DisplayDevice displayDevice1 = new DisplayDevice();
    DisplayDevice displayDevice2 = new DisplayDevice();

    weatherStation.registerObserver(displayDevice1);
    weatherStation.registerObserver(displayDevice2);

    // Simulate weather change
    weatherStation.setWeatherData(new WeatherData(/* ... */));
}
```

In this example, the WeatherStation acts as the Observable, and the DisplayDevice represents an Observer. Multiple DisplayDevices can be registered with the WeatherStation, and they will be notified whenever the weather data changes.

## Conclusion

The Observer/Observable design pattern offers a flexible and extensible approach to managing dependencies between objects. It promotes loose coupling, making it easier to modify and extend systems without impacting existing code.

*Tags: #ObserverPattern #ObservablePattern*