# React Intro

## Prior Knowledge

- Understanding and awareness of basic HTML elements
- DOM tree structure
- DOM manipulation
- Javascript

## Learning Objectives

- Understand how a single page application is structured in React.
- Be able to use JSX to create html elements.
- Be able to use JSX to insert JS values into html.

## React JS

> "A JavaScript library for building user interfaces" - [React Homepage](https://reactjs.org/)

### Why React ?

Making changes through DOM manipulation can be slow as well as difficult.

- The DOM is relatively slow to update due to it's tree structure, which requires recursion to traverse.

- React gives developers the ability to work with a _virtual DOM_, removing the need to use the browser's inbuilt DOM methods.

- A benefit of this is that it allows us to be _declarative_ when building our User Interface (UI) using JavaScript. This means that rather than having to describe each step in the transition to an updated UI using DOM methods, we can simply write the final version of the page that should be displayed with React.

- The _virtual DOM_ used by React is much faster and more efficient to update, as instead of manually traversing through the parent and child nodes of the DOM, the _virtual DOM_ will check itself against the previous version and only update what is necessary, based on the updated UI that has been declared.

- As a result, React makes it easy to "hydrate" a page with data from API requests on the client side.

## Basic React Set-up

The file structure of a typical React application will something like this:

```raw
├── README.md
├── package-lock.json
├── package.json
├── public
│   ├── favicon.ico
│   ├── index.html
└── src
    ├── App.css
    ├── App.js
    ├── index.css
    └── index.js
```

In the majority of React apps that you see and create, there will be a `div` with an `id` of `root` within an `index.html` file. Typically, `index.html` will be found inside a public folder as can be seen in the above folder structure.

```html
-- the rest of the html file...
<div id="root"></div>
-- html file continued...
```

This is referred to as the “root” DOM node because everything inside it will be managed by React, and this is where our React apps will be rendered (displayed).

Within an `index.js` file, we can render a React element inside the root DOM node using `root.render()`:

```js
import ReactDOM from 'react-dom';

const element = <h1>Hello, world</h1>;

const root = ReactDOM.createRoot(document.getElementById('root'))
root.render(element);
```

The `element` that has been created above looks just like a HTML element, but the above example is a blend of JavaScript and HTML syntax, this is in fact a syntax extension of JavaScript called JSX.

JSX effectively allows us to write HTML syntax inside our JavaScript files. See [React Intro to JSX](https://reactjs.org/docs/introducing-jsx.html) for more info.

### Babel

Notice the way that the `react-dom` package is required (or in this case _imported_) into the file. This syntax is made possible by [Babel](https://babeljs.io/repl), a JavaScript compiler, which compiles our code down to browser compatible JavaScript. The `import` syntax is modern ES6 JavaScript which is converted to Common JS `require` under the hood.

Some more details on the difference between `import` & `require` courtesy of [Stack Overflow](https://stackoverflow.com/questions/31354559/using-node-js-require-vs-es6-import-export)

Babel can also be used to show why we can create a variable that looks like an HTML element when using React:

```js
const element = <h1>Hello world</h1>;
```

Compiles down to:

```js
import { jsx as _jsx } from 'react/jsx-runtime';

const element = /*#__PURE__*/ _jsx('h1', {
  children: 'Hello world',
});
```

Here we can see that the `jsx-runtime` has been imported automatically and the element is actually a call to this function. This is why it can be saved to a variable and treated as a normal JS value.

**n.b.** This behaviour of automatically importing the `jsx-runtime` is new to React 17. Previous versions of React complied JSX to `React.createElement` instead. This required React to be in scope and as such we would have to add the following import to any file using JSX.

```js
import React from 'react';

const element = <h1>Hello world</h1>;
```

You may still see this import in projects using React versions < 17

## React Elements

JavaScript can be embedded within JSX using curly braces. E.g.

```js
const sum = <p>1 plus 2 = {1 + 2}</p>; // {1 + 2} will be evaluated to 3

const root = ReactDOM.createRoot(document.getElementById('root'))
root.render(sum);
```

See [Embedding Expressions in JSX](https://reactjs.org/docs/introducing-jsx.html#embedding-expressions-in-jsx) for more info.

We can embed several JS data types, notably `Strings`, `Numbers` and `Arrays`. `Objects` however are not valid and React will throw an error if we try to embed one. If we want to render the information from an object we should render the properties into appropriate tags.

```js
const person = {
  name: 'Paul',
  age: 34,
};

const profile = (
  <section>
    <h1>{person.name}</h1>
    <p>{person.age} years old</p>
  </section>
);

const root = ReactDOM.createRoot(document.getElementById('root'))
root.render(profile);
```

## Displaying Lists and Keys

React allows us to render arrays. Each element of the array will be rendered to the DOM in order. Good semantically correct html should be made up of an `ul` or `ol` tag containing `li` tags as their children.

A common technique is to use `.map` to transform an array of JS values into an array of `li` tags. See the React docs for more info on [Lists and Keys](https://reactjs.org/docs/lists-and-keys.html).

```js
const listItems = ['One', 'Two', 'Three', "Four o'clock rock"];

const rockSchedule = (
  <ul>
    {listItems.map((item) => {
      return <li key={item}>{item}</li>;
    })}
  </ul>
);

const root = ReactDOM.createRoot(document.getElementById('root'))
root.render(rockSchedule);
```

Each item in the array must be given a key prop. This is for React to identify each element when adding / removing or re-ordering elements in the array. The key should be stable and unique. Best practice here is to use an id or some unique feature of each element.

See the [react docs on keys](https://reactjs.org/docs/lists-and-keys.html#keys) for more detail on how they are used and why indexes should only be used as a last resort.

Dan Abramov (ReactJS dev & co-creator of Redux and Create-React-App) also posted [this fantastic twitter thread](https://twitter.com/dan_abramov/status/1415279090446204929?s=20) explaining _Why React Needs Keys_.

## ES6 Modules

In React we will use [ES6 modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) to import and export variables as opposed to the [common JS](https://nodejs.org/docs/latest/api/modules.html) modules that we used in node.

MDN has an excellent guide on how they work but some of the more common patterns are shown below:

### Default exports - exporting a single value

To export a value from a file in node we would assign that value to the `module.exports`. `ES6 modules` come with the ability to export a single `default` value which can be imported in other files.

```js
// sayHello.js
function sayHello() {
  console.log('hello world');
}

export default sayHello;

// another file
import sayHello from './sayHello.js';
```

is equivalent to

```js
function sayHello() {
  console.log('hello world');
}

module.exports = sayHello;

// another file
const sayHello = require('./sayHello.js');
```

### Named exports - exporting multiple values

When exporting multiple values in node we typically export an object containing those values and destructure them when importing. The same can be done with the `export` syntax put in front of a variable we want to export.

```js
// functions.js
export const foo = () => {};
export const bar = () => {};
export const baz = () => {};

// another file
import { foo, bar, baz } from './functions';
```

is equivalent to

```js
// functions.js
exports.foo = () => {};
exports.bar = () => {};
exports.baz = () => {};

// another file
const { foo, bar, baz } = require('./functions');
```

When importing named exports they can be renamed or collected into a single object using the `as` syntax.

```js
import * as myFunctions from './functions';

console.log(myFunctions); // { foo: [Function foo], bar: [Function bar], baz: [Function baz] }
```

### Additional features

There are some additional features of `ES6 modules` that do not have equivalents in node.

For example, you can have both a `default` export as well as several `named` exports.

```js
// myLibrary.js
function myLibrary() {}

export const helperFunction1 = () => {};
export const helperFunction2 = () => {};

export default myLibrary;

// another file
import myLibrary, { helperFunction1, helperFunction2 } from './myLibrary';
```
