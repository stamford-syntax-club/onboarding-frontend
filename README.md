# Stamford Center Frontend: Onboarding Guide

Welcome to the `stamfordcenter-frontend` project! This guide is meant to help new developers become familiar with our setup.

For you to be able to fully utilize this guide, you **must** have a solid foundation in JavaScript, and have the programmer's mindset when it comes to problem solving. If you have just passed our programming course, then you will be using everything you have learned there, and also build upon those fundamental concepts.

## Getting Started

1. **Clone the repository**:
    ```bash
    git clone [repository-url] stamfordcenter-frontend
    ```

2. **Change directory**:
    ```bash
    cd stamfordcenter-frontend
    ```

3. **Install dependencies**:
    ```bash
    npm install
    ```

## Development Workflow

- **Start the development server**:
    ```bash
    npm run dev
    ```

- **Build the project** (you will not be using this yourself often):
    ```bash
    npm run build
    ```

- **Start the production server** (you will not be using this yourself often either):
    ```bash
    npm run start
    ```

## Code Quality & Standards

- **Linting**:
    ```bash
    npm run lint
    ```

- Ensure you have the `Prettier` VS Code extension installed for code formatting.

## Technologies and Libraries

Here's an overview of the core technologies and libraries we use:

- **Framework**: Next.js (v13.4.12) (this is essentially React with a fancy hat)
- **UI Library**: Mantine
- **CSS**: TailwindCSS with PostCSS and Autoprefixer
- **Date Handling**: dayjs
- **Data Fetching**: SWR
- **Language / Type Checking**: TypeScript

## Key Dependencies:

- **Next.js**: A React framework.
- **Mantine**: A modern React component library.
- **TailwindCSS**: A utility-first CSS framework.
- **dayjs**: A lightweight JavaScript date library.
- **SWR**: React Hooks library for data fetching.
- **React Icons**: Popular icons as React components.
- **TypeScript**: Static type checking for JavaScript.
- **ESLint**: For linting and ensuring code quality.
- **Prettier**: Code formatting.

## Additional Notes

- Please refer to individual library documentation when necessary:
  - [Next.js](https://nextjs.org/)
  - [Mantine](https://mantine.dev/)
  - [TailwindCSS](https://tailwindcss.com/)
  - [dayjs](https://day.js.org/)
  - [SWR](https://swr.vercel.app/)
  - [React Icons](https://react-icons.github.io/react-icons/)
  - [TypeScript](https://www.typescriptlang.org/)

---
## Core React Concepts

This is the React section of our onboarding guide. React offers a declarative approach to building UI, and understanding its foundational concepts is essential. Here's a primer:

### 1. **Hooks**

Hooks are a fairly recent addition to React that allow you to use state and other React features without writing a class. They help simplify your components and promote the use of functional components throughout applications. Examples of commonly used hooks include `useState`, `useEffect`, and `useContext`.

### 2. **Components**

Components are the building blocks of a React application. A component can be thought of as a self-contained unit of code that renders some UI.

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

### 3. **Props**

Props (short for "properties") allow you to pass data to components. They are read-only and help make your components reusable.

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

<Welcome name="React" />
```

### 4. **State**

State in React refers to the local data a component can maintain and alter during its lifecycle. Unlike `props`, which are passed to a component and remain unchanged, state is internal to a component and can change over time, with each time it changes prompting a re-render of the component.

In functional components that we are going to be using, the `useState` hook is the standard way to introduce state management:

#### **`useState` Hook**

The `useState` hook allows functional components to use state. It returns an array with two elements:

1. The current state value.
2. A function to update that value.

Here's a breakdown:

```jsx
import React, { useState } from 'react';

function Counter() {
  // Declare a state variable named 'count' with an initial value of 0
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

In the example above:
- `count` is the current state value, initially set to `0`.
- `setCount` is the function we use to update the state. When you want to change the state, you must use this function. Directly modifying the `count` would not cause the component to re-render.

#### **State Updates are Asynchronous**

It's important to remember that calls to `setCount` or any state updater function are asynchronous. React batches these updates and handles them in a way that optimizes performance. This means you might not see the updated state immediately after calling the updater function.

For instance, if you have multiple updates in a row:

```javascript
setCount(count + 1);
setCount(count + 1);
setCount(count + 1);
```

You might expect the count to increment by 3, but it won't. Each call would base its update on the same initial value. To fix this, you can use a functional update:

```javascript
setCount(prevCount => prevCount + 1);
setCount(prevCount => prevCount + 1);
setCount(prevCount => prevCount + 1);
```

Here, each call gets the most recent state as its argument (`prevCount`), ensuring all updates are applied sequentially.

#### **Using State Correctly**

- **Do Not Modify State Directly**: Always use the updater function. E.g., this is wrong: `this.state.count = 123;` but this is correct: `setCount(123);`.
  
- **State Updates May Be Merged**: When you call the state updater function multiple times, React may batch them together and merge the updates for performance reasons. So always ensure your state updates are not dependent on their order unless you're using functional updates.

#### **State vs. Props**

While both state and props can affect the output of a rendered component, there are key differences:
- **Props** are read-only and are set by the parent component. They shouldn't be changed inside the component.
- **State** is mutable and can be changed within the component using the state updater function.

Remember, every time the state or props change, the component will re-render to reflect those changes.

### 5. **Effects**

The `useEffect` hook lets you perform side effects in functional components. Side effects might be things like data fetching, subscriptions, or manually changing the DOM.

Here's a basic example:

```jsx
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;
    
    // Cleanup logic here
    return () => {
      // For instance: clear a subscription or timer
    };
  }, [count]); // Second argument is a dependency array
}
```

The `useEffect` hook can optionally take a dependency array as its second argument. This array tells React which variables from the component context should trigger the effect when they change:
- If you pass an empty array (`[]`), the effect will only run once (similar to `componentDidMount` in class components).
- If you pass values inside the array, the effect will run every time those values change.
- If you don't pass an array, the effect will run after every render.

### 6. **Event Handling**

React provides a way to capture user interactions like clicks, form submissions, etc., by attaching event handlers to JSX elements:

```jsx
function Button() {
  function handleClick() {
    alert('Button was clicked!');
  }

  return <button onClick={handleClick}>Click Me</button>;
}
```

### 7. **Context**

Context provides a way to share values between components without having to explicitly pass a prop through every level of the tree, A.K.A prop drilling. This helps with maintainability and readability.

- `React.createContext`: Creates a new context.
- `Context.Provider`: Every context object comes with a Provider. It allows consuming components to subscribe to context changes. Accepts a `value` prop which can be passed to consuming components.
- `useContext`: Accepts a context object and returns the current value for that context.

In this example:

- The `ThemeContext` is created with a default value of 'light'.
- The `App` component uses the `ThemeContext.Provider` to pass the "dark" value down to the `Toolbar` component and its descendants.
- The `ThemedButton` component uses the `useContext` hook to consume the current theme value.

```jsx
const ThemeContext = React.createContext('light');

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar() {
  // This component doesn't need to know about the theme itself but provides a placeholder for the ThemedButton.
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

function ThemedButton() {
  const theme = useContext(ThemeContext);
  return <button theme={theme}>Styled Button</button>;
}
```

---
## **Next.js Concepts**

Next.js builds upon the fundamentals of React, providing a robust framework to streamline the development of scalable applications, especially those that need server-side rendering, static site generation, and efficient routing.

### **1. Pages & Routing**

Unlike vanilla React, where routing can be achieved via libraries like `react-router`, Next.js has a file-system based router built-in. This allows you to simply create a file and have paths created for you. 

For example, if you create a file called `test.tsx`,
```jsx
import React from 'react'

export default function TestPage() {
	return (
		<div>TestPage</div>
	)
}
```

Run `npm run dev`
Then navigate to `http://localhost:3000/test`

You will see the content of the `TestPage` component.

Keep in mind that the file name is the route name, and the default export is the component that will be rendered.

The function can be called essentially any valid function name, same goes for the file name with some caveats.

For example, `test.tsx` will be accessible via `/test`, `test-page.tsx` will be accessible via `/test-page`, and so on.

### **2. API Routes**

Next.js allows you to build API endpoints within the same project. API routes reside in the `pages/api` directory. We don't use this in Stamford Center, opting to split it into its own Express server instead, but this will serve as an introduction to API routes.

- Creating a function inside `pages/api/hello.js` will expose an endpoint as `/api/hello`.
- These routes are Node.js functions, giving you full control over the server-side logic.

### **3. Data Fetching**

One of the significant advantages of Next.js is its support for server-side rendering and static site generation, offering optimal performance and SEO.

- **getStaticProps**: Fetches data at build time and uses it to generate a static site.

- **getServerSideProps**: Fetches data on each request, rendering your page server-side.

- **getStaticPaths**: Specifies dynamic routes to pre-render based on data.

### **4. Linking Between Pages**

Next.js provides a `<Link>` component to enable client-side navigation between routes.

```jsx
import Link from 'next/link';

function Navigation() {
  return (
    <nav>
      <Link href="/">
        <a>Home</a>
      </Link>
      <Link href="/about">
        <a>About</a>
      </Link>
    </nav>
  );
}
```

### **5. Built-in CSS Support**

Next.js allows you to import CSS directly within your components. It also supports CSS Modules out of the box, ensuring styles are locally scoped to components.

### **6. Image Optimization**

With the `<Image>` component, Next.js offers automatic image optimization, resizing images on-demand, and serving them in modern formats.

### **7. Environment Variables**

Next.js has built-in support for `.env` files. By prefixing variables with `NEXT_PUBLIC_`, they are accessible in the browser.

### **8. Enhanced Development Experience**

- **Fast Refresh**: Real-time feedback during development. Save your components, see the result instantly.
  
- **Built-in ESLint**: As seen in the `package.json`, there's built-in support for linting to encourage best practices.

- **Error Reporting**: Comprehensive error messages, helping you quickly understand and resolve issues.

### **9. Extending with `next.config.js`**

Much like `webpack`, Next.js can be extended with a `next.config.js` file. Here, you can modify build steps, add plugins, and set up various configurations for your app.

---
