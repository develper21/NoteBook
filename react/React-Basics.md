# React Basics

## What is React?
React is a JavaScript library for building fast and interactive user interfaces for web and mobile applications. It works by allowing developers to create reusable UI components that update efficiently when data changes.[12]

## JSX (JavaScript XML)
JSX lets you write HTML-like code within JavaScript files. It looks like regular HTML, but under the hood, it is converted into JavaScript objects. JSX makes it easier to describe UIs and see what they will look like.[8][5]

## Components
Components are independent, reusable pieces of UI. Every React application is made of components.

- Functional Components: These are just JavaScript functions that return JSX.
- Class Components: These use classes and can have extra features; however, functional components are now preferred.[5]

## Props (Properties)
Props are how data is passed from parent to child components. They allow components to be dynamic and configurable, but child components cannot change the props directly.[5]

## State
State represents changing data inside a component. When state changes, the component re-renders automatically. For example, form input or fetched data is commonly stored in state.[7][5]

## Event Handling
React handles events like clicks and input changes using event handlers. These handlers allow you to create interactive applications.[5]

## Conditional Rendering
Using logic like if/else, you can control what JSX gets rendered depending on state or props, making your UI dynamic.[7][5]

## Lists and Keys
When displaying lists (arrays) of items, each item must have a unique key to help React track changes efficiently. Keys make lists interactive and performant.[7][5]

## Forms and Form Handling
React provides controlled components for form inputs, where input values are tied to state variables to handle and validate user input easily.[5]

## Styling in React
There are multiple ways to style React apps:
- Inline styles: JavaScript objects directly in JSX.
- CSS Stylesheets: Traditional CSS files.
- CSS Modules: Locally scoped CSS to prevent style conflicts.[11][5]

## Hooks
Hooks are functions that let you use state and other React features in functional components:
- useState: Adds state to functional components.
- useEffect: Handles side effects like data fetching and subscriptions.
- useRef: Accesses DOM elements directly.
- useContext: Shares data between components without passing props down manually.[11][7][5]

## Context API
The Context API allows you to manage and share global data across many components, such as user authentication status or themes.[11][5]

## Component Lifecycle
Components go through phases: mounting, updating, unmounting. Lifecycle methods (for class components) and hooks (in functional components) let you run code at certain points during these phases.[9][5]

## Lifting State Up
When several components need to share state, the state is moved ("lifted up") to their common ancestor and passed down as props, making data flow predictable.[5]

## React Router
React Router enables navigation and routing for single-page applications, allowing seamless page changes without reloading. Important concepts include react-router-dom, nested routes, and route parameters.[9][11][5]

## Fragments
React Fragments let you group multiple elements without adding extra nodes to the DOM, helping keep the markup clean.[7][5]

## Error Boundaries
Error boundaries catch and handle errors during rendering, providing fallback UIs and better user experience.[5]

## Code Splitting / Lazy Loading
Code splitting allows loading only necessary parts of an app to make initial load faster. React's lazy loading helps split code at component level, improving performance.[11][5]

## Higher-Order Components (HOC)
HOCs are functions that take components and enhance them by adding extra functionality, promoting code reuse.[5]

## Render Props Pattern
This pattern passes a function (prop) into components to control what is rendered, making components more flexible.[5]

## Controlled vs Uncontrolled Components
Controlled components have their data managed by React state, while uncontrolled components manage their own state internally.

## Advanced Ecosystem Topics
React integrates well with popular tools for building UI:
- Redux: Advanced state management for complex apps.
- Material UI, Bootstrap, and Tailwind: Libraries for styling and ready-made components.
- Framer Motion: Animation toolkit for smooth transitions.[9][11]

## Accessibility and Semantics
React encourages use of semantic HTML and ARIA roles for building accessible apps, making sure everyone, including users with disabilities, can use your application.[11]

## Performance Optimization
React provides techniques like memoization, virtualization, lazy loading, and efficient state management to make applications fast and responsive.[11]
---