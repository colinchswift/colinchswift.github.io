---
layout: post
title: "Integrating with front-end frameworks (React, Vue, Angular) in Swift Vapor"
description: " "
date: 2023-10-31
tags: []
comments: true
share: true
---

When developing a web application with Swift Vapor, you may need to integrate with front-end frameworks like React, Vue, or Angular to build a dynamic and interactive user interface. In this blog post, we will explore how to seamlessly integrate these popular front-end frameworks with a Swift Vapor backend.

## 1. Setting Up Your Vapor Project

To begin, create a new Vapor project by running the following command in your terminal:

```bash
vapor new MyProject
```

Navigate into the project directory:

```bash
cd MyProject
```

Initialize the Swift package manager:

```bash
swift package init --type=executable
```

## 2. React Integration

### Installing Dependencies

For integrating React, we need to install Node.js and npm (Node Package Manager) to manage the JavaScript dependencies. 

First, make sure you have Node.js installed on your system by running:

```bash
node -v
```

If Node.js is not installed, you can download it from the official website (https://nodejs.org) and follow the installation instructions.

Once Node.js is installed, navigate to your Vapor project's directory and run the following command to initialize npm:

```bash
npm init -y
```

This will create a `package.json` file.

Next, install React dependencies by running:

```bash
npm install react react-dom
```

### Creating a React Component

To create a React component, let's update the `Resources/Views/Home/template.leaf` file to include a `div` with an `id`:

```html
<div id="root">@content</div>
```

Now, create a new file `Resources/Assets/js/main.js` and import React and ReactDOM:

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render(
  <h1>Hello, React!</h1>,
  document.getElementById('root')
);
```

### Building and Serving

To build and serve your Vapor project, run the following command:

```bash
vapor build --enable-migrator run serve
```

Now, you can view your React component by visiting `http://localhost:8080` in your browser.

## 3. Vue Integration

### Installing Dependencies

Similar to React, we need to install Node.js and npm to manage the Vue dependencies. Ensure that Node.js is installed, and navigate to your Vapor project's directory.

Initialize npm by running:

```bash
npm init -y
```

Then, install Vue by running:

```bash
npm install vue
```

### Creating a Vue Component

Update the `Resources/Views/Home/template.leaf` file to include a `div` with an `id` attribute:

```html
<div id="app">@content</div>
```

Now, create a new file `Resources/Assets/js/main.js` and import Vue:

```javascript
import Vue from 'vue';

new Vue({
  el: '#app',
  template: '<h1>Hello, Vue!</h1>'
});
```

### Building and Serving

Build and serve your Vapor project:

```bash
vapor build --enable-migrator run serve
```

Visit `http://localhost:8080` in your browser to view the Vue component created.

## 4. Angular Integration

### Installing Dependencies

To integrate Angular with Swift Vapor, we need to install Node.js and npm.

Ensure that Node.js is installed, and navigate to your Vapor project's directory.

Initialize npm:

```bash
npm init -y
```

Then, install Angular by running:

```bash
npm install @angular/cli
```

### Creating an Angular Component

Generate a new Angular component by running the following command:

```bash
npx ng generate component my-component
```

This will create a new folder `Resources/Assets/app/my-component` containing the Angular component files.

### Building and Serving

Build and serve your Vapor project:

```bash
vapor build --enable-migrator run serve
```

Visit `http://localhost:8080` in your browser to view the Angular component.

## Conclusion

Integrating front-end frameworks like React, Vue, or Angular with Swift Vapor allows you to leverage the power of these frameworks while still utilizing the server-side capabilities of Vapor. By following the steps outlined in this blog post, you can seamlessly integrate these frameworks into your Vapor projects, enabling you to build dynamic and interactive web applications. Happy coding!

# References

- [Vapor Documentation](https://docs.vapor.codes/)
- [React Documentation](https://reactjs.org/docs/getting-started.html)
- [Vue Documentation](https://vuejs.org/v2/guide/)
- [Angular Documentation](https://angular.io/docs)