---
layout: post
title: "Model-View-Presenter (MVP) design pattern"
description: " "
date: 2023-10-26
tags: [designpattern]
comments: true
share: true
---

The Model-View-Presenter (MVP) design pattern is a software architecture pattern commonly used in developing user interfaces. It separates the application logic and the user interface components, making the code more maintainable, testable, and scalable.

## Table of Contents
- [Introduction to MVP Design Pattern](#introduction-to-mvp-design-pattern)
- [Components of MVP](#components-of-mvp)
  - [Model](#model)
  - [View](#view)
  - [Presenter](#presenter)
- [Benefits of Using MVP](#benefits-of-using-mvp)
- [Example Code](#example-code)
- [Conclusion](#conclusion)
- [References](#references)

## Introduction to MVP Design Pattern

The MVP design pattern is derived from the Model-View-Controller (MVC) pattern and aims to overcome some of its limitations, such as the tight coupling between the view and controller. MVP adds an extra layer, called the presenter, to mediate between the view and model.

In MVP, the view is responsible for capturing user input and displaying data, while the presenter handles the application logic and interacts with the model. The model contains the business logic and data operations.

## Components of MVP

### Model

The model represents the application's data and business logic. It encapsulates the data structures, computations, and database or API interactions. The model notifies the presenter about any changes in the data, and the presenter updates the view accordingly.

### View

The view is responsible for rendering the user interface and capturing user input events. It provides a platform to display data and convey information to the user. The view interacts with the presenter to update the UI based on the business logic.

### Presenter

The presenter acts as the middleman between the view and model. It receives user input events from the view, manipulates the data in the model, and updates the view accordingly. The presenter also listens for updates from the model and notifies the view to reflect any changes.

## Benefits of Using MVP

- **Separation of Concerns**: MVP separates the responsibilities of the view, presenter, and model, making the codebase more modular and maintainable.
- **Testability**: The MVP pattern allows for easier unit testing of the different components, as they can be tested independently without relying on the full user interface.
- **Code Reusability**: The clear separation between the view, presenter, and model enables code reuse and easier refactoring.
- **Enhanced UX**: With the presenter handling the business logic, the view can focus on providing a smooth and intuitive user experience.

## Example Code

Here's an example of how the MVP pattern can be implemented in a simple Android application:

```java
// Model
public class UserDataModel {
    private String username;
    
    public void setUsername(String username) {
        this.username = username;
    }
    
    public String getUsername() {
        return username;
    }
}

// View
public interface UserView {
    void showUsername(String username);
}

// Presenter
public class UserPresenter {
    private UserView view;
    private UserDataModel model;
    
    public UserPresenter(UserView view, UserDataModel model) {
        this.view = view;
        this.model = model;
    }
    
    public void setUsername(String username) {
        model.setUsername(username);
        view.showUsername(username);
    }
}

// Activity (View Implementation)
public class UserActivity extends AppCompatActivity implements UserView {
    private UserPresenter presenter;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_user);
        
        UserDataModel model = new UserDataModel();
        presenter = new UserPresenter(this, model);
        
        // Capture user input and update the model via presenter
        EditText usernameInput = findViewById(R.id.username_input);
        Button saveButton = findViewById(R.id.save_button);
        
        saveButton.setOnClickListener(v -> {
            String username = usernameInput.getText().toString();
            presenter.setUsername(username);
        });
    }
    
    @Override
    public void showUsername(String username) {
        // Update the view with the new username
        TextView usernameTextView = findViewById(R.id.username_textview);
        usernameTextView.setText(username);
    }
}
```

In this example, the `UserDataModel` represents the data and the `UserPresenter` handles the business logic. The `UserView` interface defines the contract for the view implementation (`UserActivity`). The presenter updates the view by calling the `showUsername` method.

## Conclusion

The Model-View-Presenter (MVP) design pattern provides a structured approach to building user interfaces by separating the concerns of the model, view, and presenter. By adopting MVP, developers can achieve cleaner code, enhanced testability, and improved code reusability.

## References

1. [Model-View-Presenter (MVP) Pattern - Baeldung](https://www.baeldung.com/mvp-pattern) #mvp #designpattern