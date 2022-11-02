# Arrays & Iteration

## Learning Objectives

- understand that arrays are ordered lists of values
- know how to declare arrays in JavaScript
- understand how `for` loops are used in JavaScript
- know how to use for loops and for of loops to iterate over arrays

## Primitives

We have already seen that JavaScript has 7 primitive data-types : number, string, boolean, undefined, null, symbol and bigint. These data types are the fundamental types we use in the JavaScript. However, it would become increasingly difficult to manipulate groups of data if the language only permitted the use of primitive data types.

## Storing multiple items

Consider a situation where we want to create multiple variables that represents a **list of data**:

```js
const item0 = 'apple';
const item1 = 'banana';
const item2 = 'strawberry';
```

We could continue to create variables in this way: however, we would have to keep declaring new variables every time we wanted to add something to this list. Instead we can think of storing this data in a single **data structure**

## Arrays

> A JavaScript array is an **ordered collection of data**.

### Declaring arrays

JavaScript arrays are written using `[` and `]` with each item inside the square brackets separated by a comma. We can create an array in the following way:

```js
const items = ['apple', 'banana', 'strawberry'];
```

`items` is an array containing 3 **elements**, an **element** being an item inside the array. The strings, `"apple"`,`"banana"` and `"strawberry"` are said to be the **3 elements** making up the array `items`.

---

### Zero indexing

In computer science, **zero indexing** means that we start counting ordered collections from **zero**. All JavaScript arrays are **zero-indexed**. We can use the index position together with square bracket notation in order to access an array element at a specific position:

```js
const letters = ['a', 'b', 'c'];
letters[0]; // evaluates to 'a'
letters[1]; // evaluates to 'b'
```

An array can be accessed with a position that is bigger than the array length: however, in this case the access will evaluate to `undefined`:

```js
const letters = ['a', 'b', 'c'];
// only 3 items

letters[5]; // evaluates to undefined
```

---

### `.length`

All arrays have a `.length` property indicating the number of items present inside the array. In other languages, arrays and ordered collections of data have limitations on the amount of data they can store. In JavaScript, the physical limitations of an array length are usually far beyond what will normally be needed. In addition, Javascript arrays can hold items of multiple data types, this makes them extremely flexible **data structures**. The `.length` property of an array will be updated when an arrays contents are altered. Javascript arrays are said to be **mutable** as we can update their contents over time.

```js
const items = ['apple', 'banana', 'strawberry'];
items.length; // will be 3

items[3] = 'kiwi'; // this will insert an item "kiwi" at index position 3
items.length; // will now be 4
```

### Methods

We will frequently want to update arrays by adding or removing items.

JavaScript arrays come with some inbuilt methods (i.e. functions) that can be chained onto arrays themselves:

`.push()` is a **mutating array method**. It will mutate an array by adding an item to the end of an array.

```js
const letters = ['a', 'b', 'c'];
letters.push('d');
// letters is now ["a", "b", "c", "d"];
```

`.pop()` is a **mutating array method** that acts by removing an item from the end of an array.

```js
const letters = ['a', 'b', 'c'];
const lastLetter = letters.pop(); // lastLetter is now 'c'
// letters is now ["a", "b"];
```

`.shift()` and `unshift()` work similarly to `push` and `pop`, but act at the beginning of the array:

```js
const letters = ['a', 'b', 'c'];

// shift to remove an item from the front of an array, and return its value
const firstLetter = letters.shift(); // firstLetter is now 'a'
// letters is now ["b", "c"];

// unshift to add a value to the front of an array
helpers.unshift('z');
// letters is now ["z", "b", "c"];
```

There are many more methods listed in the [Arrays MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#)

### Nested arrays

Arrays can also contain other arrays. For example:

```js
const bestTrilogies = [
  ['fellowship', 'twin towers', 'return of the king'],
  ['a new hope', 'empire strikes back', 'return of the jedi'],
  ['godfather I', 'godfather II', 'godfather III']
];

const episode4 = bestTrilogies[1][0]; // 'a new hope'
```

## Iteration

It is common that the same code will need to be executed multiple times, or for multiple items in an array. To solve this problem we can use iteration, or loops.

## For Loops

A `for loop` is a powerful tool that allows a block of code to be executed multiple times.

This is often to iterate (go through) an array or string, or to a certain number, and do something at each current index (position) of the element.

Here's an example of a `for loop` that will `console.log` each name in the `names` array.

```js
const names = ['Harry', 'Sally', 'Bob'];
for (let i = 0; i < names.length; i++) {
  console.log(names[i]);
}
```

The first part of the `for` loop (`let i = 0;`) is saying start counting from the very beginning of the array. Strings and arrays are 0 indexed in Javascript which means that we start counting from 0 (_the initial expression_).


The second part (`i < names.length;`) is saying to keep going until you've reached the end of the array (the _condition expression_) which works by running the `for` loop until the condition is no longer `true`.

The final part (`i++;`) is making sure that we go through each element of the array one by one (the _increment expression_) which increases the value of `i` in steps until the condition is no longer `true`.

If I wanted to access the very first index in the names array, outside of using a for loop, we would use square brackets to do this.

```js
name[0]; // 'Harry'
name[1]; // 'Sally'
name[2]; // 'Bob'
name[4]; // undefined
```

Inside of the for loop we can do the same thing but instead of going through an having to access each individual index, we can use the `i` index variable to access every element we need as we have already made sure that we can do that with the for loop.

For loops can also work with numbers and strings, for example:

To `console.log` every number from 0, up to and including 5

```js
const numberFive = 5;

for (let i = 0; i <= numberFive; i++) {
  console.log(i);
}

//output
// 0
// 1
// 2
// 3
// 4
// 5
```

We can't use the `.length` property here because that only exists on arrays and strings.

Or to `console.log` each letter in a string:

```js
const name = 'Bob'
for (let i = 0; i < name.length; i++) {
  console.log(name[i])
}
//output
// "B"
// "o"
// "b"
```

## for ... of Loops

For of loops are similar, but expose the items inside an array or string in order, rather than giving you an integer that increments.

```js
const name = 'Harry'; // would also work with an array of: ['H', 'a', 'r', 'r', 'y']
let capitalName = [];

for (const letter of name) {
  const capitalLetter = letter.toUpperCase();
  capitalName.push(capitalLetter);
}
console.log(capitalName);
```

If you don't need the flexibility of the for loop's conditions, initial statement and manner of incrementing, the `for ... of` loop can simplify the code.
