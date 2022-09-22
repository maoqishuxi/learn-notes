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

### 2. Hooks at a Glance(钩子一览)

Declaring multiple state variables

You can use the State Hook more than once in a single component:

```
function ExampleWithManyStates() {
  // Declare multiple state variables!
  const [age, setAge] = useState(42);
  const [fruit, setFruit] = useState('banana');
  const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);
  // ...
}
```

Hooks are functions that let you "hook into" React state and lifecycle features from function components. Hooks don't work inside classes -- they let you use React without classes. (Hook 不能用于class类)

**Effect Hook**

You've likely performed data fetching, subscriptions, or manually changing the DOM from React components before. We call these operations "side effects" (or "effects" for short) because they can affect other components and can't be done during rendering. (像获取数据，子脚本或从React组件改变DOM等，这类无法在渲染期间完成的操作)

The Effect Hook, `useEffect` , adds the ability to perform side effects from a function component. (增加函数组件执行副作用的能力)

For example, this component sets the document title after React updates the DOM:

```
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  // Similar to componentDidMount and componentDidUpdate:
  useEffect(() => {
    // Update the document title using the browser API
    document.title = `You clicked ${count} times`;
  });

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

When you call `useEffect` , you're telling React to run your "effect" function after flushing changes to the DOM. Effects are declared inside the component so they have access to its props and state. By default, React runs the effects after every render -- including the first render.

```
function FriendStatusWithCounter(props) {
  const [count, setCount] = useState(0);
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });

  const [isOnline, setIsOnline] = useState(null);
  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }
  // ...
```

**Rules of Hooks**

Hooks are JavaScript functions, but they impose two additional rules:

- Only call Hooks **at the level** . Don't call Hooks inside loops, conditions, or nested functions.
- Only call Hooks **from React function components** . Don't call Hooks from regular JavaScript functions. (There is just one other valid place to call Hooks -- your own custom Hooks. We'll learn about them in a moment.)

**Building Your Own Hooks**

Earlier on this page, we introduced a `FriendStatus` component that calls the `useState` and `useEffect` Hooks to subscribe to a friend's online status. Let's say we also want to reuse this subscription logic in another component.

First, we'll extract this logic into a custom Hook called `useFriendStatus` :

```
import React, { useState, useEffect } from 'react';

function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
    };
  });

  return isOnline;
}
```

Now we can use it from both components:

```
function FriendStatus(props) {
  const isOnline = useFriendStatus(props.friend.id);

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```

```
function FriendListItem(props) {
  const isOnline = useFriendStatus(props.friend.id);

  return (
    <li style={{ color: isOnline ? 'green' : 'black' }}>
      {props.friend.name}
    </li>
  );
}
```

The state of each component is completely independent. Hooks are a way to reuse *stateful logic* , not state itself. In fact, each *call* to a Hook has a completely isolated state -- so you can even use the same custom Hook twice in one component.

Custom Hooks are more of a convention than a feature. If a function's name starts with "`use`" and it calls other Hooks, we say it is a custom Hook. The `useSomething` naming convention is how our linter plugin is able to find bugs in the code using Hooks. (如果一个函数用use开头，我们就叫它自定义Hook函数)

### 3. Using the State Hook

**What does calling `useState` do?** It declares a "state variable". Our variable is called `count` but we could call it anything else, like `banana` . This is a way to "preserve" some values between the function calls -- `useState` is a new way to use the exact same capabilities that `this.state` provides in a class. Normally, variables "disappear" when the function exits but state variables are preserved by React.

**What do we pass to `useState` as an argument?** The only argument to the `useState()` Hook is the initial state. Unlike with classes, the state doesn't have to be an object. We can keep a number or a string if that's all we need. In our example, we just want a number for how many times the user clicked, so pass `0` as initial state for our variable. (If we wanted to store two different values in state, we would call `useState()` twice.)

**What does `useState` return?** It returns a pair of values: the current state and a function that updates it. This is why we write `const [count, setCount] = useState()` . This is similar to `this.state.count` and `this.setState` in a class, except you get them in a pair. If you're not familiar with the syntax we used, we'll come back to it [at the bottom of this page](https://reactjs.org/docs/hooks-state.html#tip-what-do-square-brackets-mean).

```
import React, { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);
```

**Reading State**

When we want to display the current count in a class, we read `this.state.count` :

```
  <p>You clicked {this.state.count} times</p>
```

In a function, we can use `count` directly:

```
  <p>You clicked {count} times</p>
```

**Updating State**

In a class, we need to call `this.setState()` to update the `count` state:

```
  <button onClick={() => this.setState({ count: this.state.count + 1 })}>
    Click me
  </button>
```

In a function, we already have `setCount` and `count` as variables so we don't need `this` :

```
  <button onClick={() => setCount(count + 1)}>
    Click me
  </button>
```

### 4. Using the Effect Hook

Data fetching, setting up a subscription, and manually the DOM in React components are all examples of side effects. Whether or not you're used to calling these operations "side effects" (or just "effects"), you've likely performed them in your components before.

**Effects with Cleanup**

Earlier, we looked at how to express side effects that don't require any cleanup. However, some effects do. For example, **we might want to set up a subscription** to some external data source.

```
import React, { useState, useEffect } from 'react';

function FriendStatus(props) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    // Specify how to clean up after this effect:
    return function cleanup() {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```

### 5. Rules of Hooks

**Only Call Hooks at the Top Level**

**Don't call Hooks inside loops, conditions, or nested functions.** Instead, always use Hooks at the top level of your React function, before any early return. By following this rule, you ensure that Hooks are called in the same order each time a component renders.

**Only Call Hooks from React Functions**

**Don't call Hooks from regular JavaScript functions.** Instead, you can:

- - [x] Call Hooks from React function components.
- - [x] Call Hooks from custom Hooks (we'll learn about them on the next page.)

**Explanation**

As we learned earlier, we can use multiple State or Effect Hooks in a single component:

```
function Form() {
  // 1. Use the name state variable
  const [name, setName] = useState('Mary');

  // 2. Use an effect for persisting the form
  useEffect(function persistForm() {
    localStorage.setItem('formData', name);
  });

  // 3. Use the surname state variable
  const [surname, setSurname] = useState('Poppins');

  // 4. Use an effect for updating the title
  useEffect(function updateTitle() {
    document.title = name + ' ' + surname;
  });

  // ...
}
```

So how does React know which state corresponds to which `useState` call? The answer is that **React relies on the order in which Hooks are called.** Our example works because the order of the Hook calls is the same on every render:

```
// ------------
// First render
// ------------
useState('Mary')           // 1. Initialize the name state variable with 'Mary'
useEffect(persistForm)     // 2. Add an effect for persisting the form
useState('Poppins')        // 3. Initialize the surname state variable with 'Poppins'
useEffect(updateTitle)     // 4. Add an effect for updating the title

// -------------
// Second render
// -------------
useState('Mary')           // 1. Read the name state variable (argument is ignored)
useEffect(persistForm)     // 2. Replace the effect for persisting the form
useState('Poppins')        // 3. Read the surname state variable (argument is ignored)
useEffect(updateTitle)     // 4. Replace the effect for updating the title

// ...
```

As long as the order of the Hook calls is the same between renders, React can associate some local state with each of them. But what happens if we put a Hook call (for example, the `persistForm` effect) inside a condition?

```
  // 🔴 We're breaking the first rule by using a Hook in a condition
  if (name !== '') {
    useEffect(function persistForm() {
      localStorage.setItem('formData', name);
    });
  }
```

The `name !== ''` condition is `true` on the first render, so we run this Hook. However, on the next render the user might clear the form, making the condition `false` . Now that we skip this Hook during rendering, the order of the Hook calls becomes different:

```
useState('Mary')           // 1. Read the name state variable (argument is ignored)
// useEffect(persistForm)  // 🔴 This Hook was skipped!
useState('Poppins')        // 🔴 2 (but was 3). Fail to read the surname state variable
useEffect(updateTitle)     // 🔴 3 (but was 4). Fail to replace the effect
```

**This is why Hooks must be called on the top level of our components.** If we want to run an effect conditionally, we can put that condition *inside* our Hook:

```
  useEffect(function persistForm() {
    // 👍 We're not breaking the first rule anymore
    if (name !== '') {
      localStorage.setItem('formData', name);
    }
  });
```

### 6. Building Your Own Hooks

**Extracting a Custom Hook**

**A custom Hook is a JavaScript function whose name starts with "use" and that may call other Hooks.** For example, `useFriendStatus` below is our first custom Hook:

```
import { useState, useEffect } from 'react';

function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
    };
  });

  return isOnline;
}
```

**Using a Custom Hook** 

