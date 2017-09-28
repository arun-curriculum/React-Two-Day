# React JS Two-Day

## Introduction to React JS

- React JS is a front-end framework, and is oftentimes referred to as simply a library.
- It was created and is still heavily maintained by Facebook.
- React helps you build front end systems that perform client-side rendering based on data that is received in JSON format from a back end.
- Here is a diagram about how this operates:

![React Diagram](img/react_diagram.jpg)

- The important takeaway is that React receives data from the backend system that is then integrated into the page view as a part of a "diff".
- It performs this via a concept that the team calls a "virtual DOM":

![Virtual DOM](img/virtual_dom.png)

- Essentially React creates a JavaScript representation of the DOM and is then responsible for creating and updating the "real" DOM itself.
- This concept increases React's performance dramatically.

## Create React App

- Create React App is a tool developed by Facebook that gets you up and running with React quickly.
- Since React uses this virtual DOM concept and does not actually run HTML and JavaScript the standard way we are used to running them, advanced build processes are necessary.
- Create React App uses Webpack, which is a popular tool to automate build steps.
- Let's install the dependencies:
    - Install Node JS: https://nodejs.org/en
    - Install Create React App: `npm install create-react-app -g`
- We can now use CRA to implement a "hello world" application: `create-react-app hello_world`.
- It is also common to "eject", which simply means to extract all of the configuration files and place them into the project folder instead of solely taking the defaults: `npm run eject`.