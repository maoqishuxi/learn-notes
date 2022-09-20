# React Tutorial

[TOC]

## MAIN CONCEPTS

### 1. Hello World

The smallest React example looks like this:

```
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<h1>Hello, world!</h1>);
```

### 2. Introducing JSX

- **JSX variable declaration**

```
const element = <h1>Hello, world!</h1>;
```

- **Embedding Expressions in JSX**

```
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;
```

You can put any valid `JavaScript expression` inside the curly braces in JSX.

```
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);
```

**JSX is an Expression Too**

This means that you can use JSX inside of `if` statements and `for` loops, assign it to variables, accept it as arguments, and return it from functions:

```
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

**Specifying Attributes with JSX**

You may use quotes to specify string literals as attributes:

```
const element = <a href="https://www.reactjs.org"> link </a>;
```

You may also use curly braces to embed a JavaScript expression in an attribute:

```
const element = <img src={user.avatarUrl}></img>;
```

You should either use quotes (for string values) or curly braces (for expressions), but not both in the same attribute.

```
tips
React DOM uses cameCase property naming.
```

**Specifying Children with JSX**

if a tag is empty, you may close it immediately with `/>` , like XML:

```
const element = <img src={user.avatarUrl} />;
```

**JSX tags may contain children:**

```
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

**JSX Prevents Injection Attacks**

It is safe to embed user input in JSX:

```
const title = response.potentiallyMaliciousInput;
// This is safe:
const element = <h1>{title}</h1>;
```

**JSX Represents Objects**

Babel compiles JSX down to `React.createElement()` calls.

```
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

```
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

### 3. Rendering Elements

Elements are the smallest building blocks of React apps.

```
const element = <h1>Hello, world</h1>;
```

Rendering an Element into the DOM

To render a React element, first pass the DOM element to `ReactDOM.createRoot()` , then pass the React element to `root.render()` :

```
const root = ReactDOM.createRoot(
  document.getElementById('root')
);
const element = <h1>Hello, world</h1>;
root.render(element);
```

**Updating the Rendered Element**

React elements are `immutable` .(React 元素是不可变对象) Once you create an element, you can't change its children or attributes.(不能改变已经创建的元素属性) An element is like a single frame in a movie: it represents the UI at a certain point in time.

With our knowledge so far, the only way to update the UI is to create a new element, and pass it to `root.render()` . (更新UI只能通过创建新的元素)

```
const root = ReactDOM.createRoot(
  document.getElementById('root')
);

function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  root.render(element);
}

setInterval(tick, 1000);
```

**React Only Updates What's Necessary**

React DOM compares the element and its children to the previous one, and only applies the DOM updates necessary to bring the DOM to the desired state. (React DOM 会比较之前的元素，只更新需要更新的部分)

### 4. Components and Props

Components let you split the UI into independent, reusable pieces, and think about each piece in isolation. (组件让您可以将 UI 拆分为独立的、可重用的部分，并单独考虑每个部分)

**Function and Class Components**

The simplest way to define a component is to write a JavaScript function:

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

You can also use an `ES6 class` to define a component:

```
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

**Rendering a Component**

Previously, we only encountered React elements(React 元素) that represent DOM tags: 

```
const element = <div />;
```

However, elements can also represent user-defined components: (元素也可以代表用户定义的组件)

```
const element = <Welcome name="Sara" />;
```

For example, this code renders "Hello, Sara" on the page:

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const root = ReactDOM.createRoot(document.getElementById('root'));
const element = <Welcome name="Sara" />;
root.render(element);
```

**Composing Components**(组合组件)

For example, we can create an `App` component that renders `Welcome` many times:

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}
```

**Props are Read-Only**

Whether you declare a component `as a function or a class` , it must never modify its own props. (声明一个组件作为一个函数或一个类，它必须不能修改自己的props)

Such functions are called `"pure"` because they do not attempt to change their inputs, and always return the same result for the same inputs. (像这样不改变它们输入的函数叫做纯函数)

**All React components must act like pure functions with respect to their props.** (所有 React 组件就其 props 而言必须像纯函数一样工作)

### 5. State and Lifecycle(状态和生命周期)

This page introduces the concept of state and lifecycle in a React component.

......

### 6. Handling Events

Handling events with React elements is very similar to handling events on DOM elements. There are some syntax differences: 

- React events are named using camelCase, rather than lowercase.
- With JSX you pass a function as the event handler, rather than a string.

For example, the HTML:

```
<button onclick="activateLasers()">
  Activate Lasers
</button>
```

is slightly different in React:

```
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

Another difference is that you cannot return `false` to prevent default behavior in React. You must call `preventDefault` explicitly. (阻止默认浏览器行为)

```
<form onsubmit="console.log('You clicked submit.'); return false">
  <button type="submit">Submit</button>
</form>
```

In React, this could instead be:

```
function Form() {
  function handleSubmit(e) {
    e.preventDefault();
    console.log('You clicked submit.');
  }

  return (
    <form onSubmit={handleSubmit}>
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Passing Arguments to Event Handlers**

Inside a loop, it is common to want to pass an extra parameter to an event handler. For example, if `id` is the row ID, either of the following would work:

```
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

### 7. Conditional Rendering

In React, you can create distinct components that encapsulate behavior you need. Then, you can render only some of them, depending on the state of your application.

We'll create a `Greeting` component that displays either of these components depending on whether a user is logged in:

```
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}

const root = ReactDOM.createRoot(document.getElementById('root')); 
// Try changing to isLoggedIn={true}:
root.render(<Greeting isLoggedIn={false} />);
```

**Inline If with Logical && Operator**

You may `embed expressions in JSX` by wrapping them in curly braces. This includes the JavaScript logical `&&` operator. It can be handy for conditionally including an element:

```
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}

const messages = ['React', 'Re: React', 'Re:Re: React'];

const root = ReactDOM.createRoot(document.getElementById('root')); 
root.render(<Mailbox unreadMessages={messages} />);
```

**Inline If-Else with Conditional Operator**

Another method for conditionally rendering elements inline is to use the JavaScript conditional operator `condition ? true : false` .

```
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
    </div>
  );
}
```

### 8. Lists and Keys

First, let's review how you transform lists in JavaScript.

Given the code below, we use the [`map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) function to take an array of `numbers` and double their values. We assign the new array returned by `map()` to the variable `doubled` and log it:

```
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map((number) => number * 2);
console.log(doubled);
```

**Rendering Multiple Components**

You can build collections of elements and `include them in JSX` using curly braces {}.

Below, we loop through the `numbers` array using the JavaScript `map()` function. We return a `<li>` element for each item. Finally, we assign the resulting array of elements to `listItems` :

```
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li>{number}</li>
);
```

**Basic List Component**

Usually you would render lists inside a component. (在组件内渲染列表)

We can refactor the previous example into a component that accepts an array of `number` and outputs a list of elements.

```
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>{number}</li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<NumberList numbers={numbers} />);
```

## HOOKS

### 1. Introducing Hooks

Hooks are a new addition in React 16.8. They let you use state and other React features without writing a class.

```
import React, { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

### 2. Hooks at a Glance

