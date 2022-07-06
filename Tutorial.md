# React Context for Beginners.

React context is a very valuble tool. However, it can be complex for beginners to understand and utilise to it's full potential without miss using it.
Just this morning I was as confused as you are now. In this tutorial we are going to answer the following questions: "What is it?", "When should I use it?", "How do I use it?" and "When should I *not* use it?".

-----

## Table of contents

1. [What is React Context?](#what-is-react-context)
2. [When to use React Context](#when-to-use-react-context)
3. [How to use React Context](#how-to-use-react-context)
4. [Why you might not need Context](#why-you-might-not-need-context)
5. [Authors](#authors)

-----

## What is React Context?
#### Props
Before we understand context you need to understand props (if you already do feel free skip this section).

Props are arguments passed into React components. They are passed to components via HTML attributes. React Props are like function arguments in JavaScript and attributes in HTML.

Props are passed in to compents like HTML attributes like this example:

```javascript
// sends a userName prop to the Profile component
<Profile userName={"pattisoj"}/>
```
This prop can then be used within the component itself like in a javascript function:
```javascript
// the p tag will read "pattisoj"
function Profile ({userName}){
    return <p>{userName}</p>;
}
```
Props can also be accessed via the props object like so:
```javascript
// sends both a userName prop 
// and a colour prop to the Profile component
<Profile userName={"ClaudiaGC1339"}, colour={"Blue"}/>

// the first p tag will read "ClaudiaGC1339"
// the second p tag will read "Favourite colour: Blue"
function Profile (props){
    return 
        <div>
            <p>{props.userName}</p>
            <p>Favourite colour: {props.colour}</p>
        </div>;
}
```
I am sure you can imagine a situation where using many props on many levels may become confusing; this is where React Context comes in.


#### Context

React context allows us to pass down and use (consume) data in whatever component we need in your React app without using props. In other words, React context allows us to share data across our components more easily.

-----
## When to use React Context

React context is best used when you are passing data that can be used in any component in your application.

### These type of data include: 

- Theme data (like light or dark mode)
- User data (the currently authenticated user)
- Location-specific data (like user language)

### What problem does it aim to solve?
The main problem that React Context aims to solve is to reduce props drilling. Props drilling is when you hand down props through multiple levels of components to reach a specific nested component. The problem with this approach is that most of the components through which this data is passed have no actual need for this data. They are simply used as mediums for transporting this data.

In the below example the theme prop is passed through the app into the Navbar. It is then passed into the child components of Navbar.

```javascript
export default function App({theme}) {
  return (
    <>
      <Navbar theme={theme} />
      <Homepage theme={theme} />
      <Form theme={theme} />
      <Footer theme={theme} />
    </>
  );
}

function Navbar({ theme }) {
  return (
    <>
      <User theme={theme} />
      <Login theme={theme} />
      <Menu theme={theme} />
    </>
  );
}
```
**So what is the issue here?**

In this example the issue is that the Navbar component doesn't directly need the theme prop and it just serves as a transporter for the prop to it's children.

It would therefore be better for the child components (User, Login and Theme) to consume that prop directly.

This is the exact benifit of using React Context.

-----
## How to use React Context

There are 4 main steps to using conext -

1. Create context using the createContext method.
2. Take your created context and wrap the context provider around your component tree.
3. Put any value you like on your context provider using the value prop.
4. Read that value within any component by using the context consumer.

This may initially sound complicated, but as we break it down in the next code snippets it will become much more simple. 

Firstly we import React. Then above our `App` component, we are creating context with `React.createContext()` and putting the result in a variable, `UserContext`. In almost every case, you will want to export it as we are doing here because your component will be in another file. Note that we can pass an initial value to our value prop when we call `React.createContext()`.
```javascript
import React from 'react';

export const UserContext = React.createContext();

export default function App() {
  return (
    <>
    <UserContext.Provider value="pattisoj">
      <User />
    </UserContext.Provider>
    </>
  );
}
```
In our `App` component, we are using `UserContext`. Specifically `UserContext.Provider`. The created context is an object with two properties: `Provider` and `Consumer`, both of which are components. To pass our value down to every component in our App, we wrap our Provider component around it (in this case, `User`).
On `UserContext.Provider`, we put the `value` that we want to pass down our entire component tree. We set that equal to the `value prop` to do so. In this case, it is a username ("pattisoj").

Next we need to `consume` that context. One way of consuming context is with the useContext hook. This only became available in `React 16.8` with the arrival of React hooks.

```javascript
function User() {
  const value = React.useContext(UserContext);  
    
  return <h1>{value}</h1>;
}
```

We use the `React.useContext()` hook in our `User` component and store the value it has made available (in this case "pattisoj") in a variable which here is named `value`. We can then use that variable in our component.

*In our opinion the benefit of the useContext hook is that it makes your components more concise and allows you to create our own custom hooks.*

-----

## Why you might not need Context 

- Smaller apps don’t have to worry about the negative side-effects of using React Context. But in larger projects, with frequent state changes, the tool creates more problems than it helps solve. 
- A simple mistake in the code could cause countless rerenders, which would eventually lead to significant performance issues.
- We want to avoid passing multiple props through components that don't need it.
- See if you can better organize your components to avoid prop-drilling.

-----
## Alternatives

### State

setState() schedules an update to a component’s state object. When state changes, the component responds by re-rendering. useState always ensures the call always uses the most updated version of state. It is a form of "state management" which means it has ways to: 

- *store* an initial value
- *read* the current value
- *update* the value

### Hooks 

React has various hook that can be use in state management: **useState & useReducer**. With both of these hooks, you can: 

- store initial values by calling the hook 
- read the current value, also in that same hook call 
- update a value by calling a setState or dispatch function

**React Context does not meet any of those criteria, therefore it is *not* a state management tool**

----
## Use Case Summary

Recap the use cases:

 - ### **Context**
    - Passing down a value to nested components (from parent to child) without prop-drilling
- ### **useReducer**
    - Slightly complex component state management using a reducer function
- ### **Context + useReducer**
    - Slightly complex component state management *and* passing a state value down to the nested components without the risk of prop-drilling. 

-----
## Authors

- [@ClaudiaGonzalez](https://www.github.com/ClaudiaGC1339)
- [@JoshPattison](https://www.github.com/pattisoj)


