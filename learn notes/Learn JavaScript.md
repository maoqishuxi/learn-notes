# Learn JavaScript

## VS Code Extensions

For JavaScript Developers

- `ESLint` :
  Find and fix problems in your JavaScript code before it hits production.
- `Prettier` :
  Automatic code formatter, so you don't have to care about that extra space.
- `JavaScript (ES6) code snippets` :
  Avoid redundant typing with shortcuts to most used code fragments.

## Install Node.js using NVM

Manage updates and multiple environments

For macOS, Linux or WSL users

[https://github.com/nvm-sh/nvm](https://github.com/nvm-sh/nvm)

For Windows users

[https://github.com/coreybutler/nvm-windows](https://github.com/coreybutler/nvm-windows)

```
nvm install node
```

## Declaring variables

Three ways:

```
var one = 1;
let two = 2;
const three = 3;
```

## Working with strings

### Template Literals

**Flexible Formatting**

Syntax makes strings easier to format and read

Use placeholders ${} for variables or expressions

Respects line breaks. Doesn't need newline character "\n"

**Use backticks**

Template literals only require the backtick character ``, placed at the beginning and end of a string. No need for quote characters.

![image-20220928113622453](.\assets\image-20220928113622453.png)

## Data types in JavaScript

JavaScript is Weakly Typed

Simple type system

​		Number (float), String, Boolean, Date, Function, Array and Object

Special Types

​		`NaN`, null, undefined

**Checking Type**

<span style="color:red;">typeof</span> operator

​		Returns a string of the data type primitive

<span style="color:red;">instanceof</span> operator

​		Returns a Boolean of if a value matches the data type

**Equality Gotchas**

```
let x = 0 == '';  // true, type coerced
let x = 0 === '';  // false, type respected
```

![snipaste](.\assets\Snipaste_2022-09-28_12-29-59.png)

## Additional Mathematical Operations

```
let num1 = 100;
console.log(num1 % 1500);	//  Remainder
console.log(++num1);	// increment
console.log(--num1);	// Decrement
```

## Converting between numbers and strings

<span style="color:red;font-weight:bold;">parseInt() and parseFloat()</span>

Convert numerical strings to numbers

Adding non-numerical characters can have unintended results

`parseFloat()` is for floating point numbers, numbers with decimal points 

<span style="color:red;font-weight:bold;">toString()</span>

Convert numbers to numerical strings

![image-20220928125208121](.\assets\image-20220928125208121.png)

![image](.\assets\Snipaste_2022-09-28_12-56-12.png)

![image-20220928130051500](.\assets\image-20220928130051500.png)
