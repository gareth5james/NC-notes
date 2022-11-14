# Recursion 102 - Traversing Objects

## Prior Knowledge

- Understand key features of recursive functions.
- Understand and identify situations where recursion is advisable / necessary

## Learning Objectives

- Be more confident in applying recursion to nested collections
- Use VSCode's debugger to debug recursive problems

## A Use Case For Recursion

A common use case for recursion is to query, analyse or manipulate nested collections (arrays and objects), and tree-like structures. An example of this is the Document Object Model in the browser. `HTML` has a tree-like structure - all of the elements may have siblings and children nested to an unknown level.

## An Example: Counting Deeply Nested Arrays

The Challenge: Count the number of arrays in an arbitrarily nested array.

Always consider:

- a base case
- a step towards the base case
- what should happen on each recursive call

The best way to think about solving this problem is to scaffold out what the function should do for some simple inputs.

```javascript
function countArrays(arr) {
  let numberOfArrays = 1;
  for (let i = 0; i < arr.length; i++) {
    if (Array.isArray(arr[i])) numberOfArrays++;
  }
  return numberOfArrays;
}

countArrays([1, 2, 3]); // 1
countArrays([1, 2, 3, [5], 6, [7]]); // 3
```

Now re-use this function recursively. Instead of incrementing our counter by 1 each time we find a nested array, we need to be able to pass any arrays we encounter back to `countArrays` to be iterated through, such is with this input, where we should expect a total of 4.

`[1, 2, 3, [5], 6, [7, 8, [1, 2, 3]], 9]`

```javascript
function countArrays(arr) {
  let numberOfArrays = 1;
  for (let i = 0; i < arr.length; i++) {
    if (Array.isArray(arr[i])) numberOfArrays += countArrays(arr[i]);
  }
  return numberOfArrays;
}
```

## Passing Variables with Recursion

Our examples so far have shown us building up or totalling a result as the recursive invocations return after the base case. Sometimes it is more appropriate to pass your 'accumulated' result through to your next recursive call, so the entire object can be returned through the call stack when the base case is finally reached.

Contrast these solutions for `countWhiteSpace()`:

```js
function countWhiteSpace(str) {
  if (str === '') return 0;
  return str[0] === ' '
    ? 1 + countWhiteSpace(str.slice(1))
    : countWhiteSpace(str.slice(1));
}
```

or

```js
function passedWhiteSpace(str, count = 0) {
  if (str === '') return count;
  if (str[0] === ' ') count++;
  return passedWhiteSpace(str.slice(1), count);
}
```

This approach plays on our tendency to want to keep a counter in such situations. In a recursive function, it is pointless to declare a counter within the function scope, as it will reset on each invocation.

The solution is to keep track of our count by passing it as an argument from one execution context to the next.
We use this approach to build a result on each call and when we hit the base case, the return value will be the number of spaces. For the sake of the first invocation, we pass a default parameter (`count = 0`) so their is no type error from incrementing and it's not necessary to pass in an initial value.

A further approach would be to define a counter _out_ of scope. To do this in the global would be exceedingly bad practice as this would render our function impure, expose its behaviour to bugs, and make it difficult to test. Alternatively, you could keep the count in the scope of a helper function and modify it from within the recursive function.

```js
function helperWhiteSpace(str) {
  let count = 0;
  recursiveCount(str);
  return count;

  function recursiveCount(str) {
    if (str === '') return;
    if (str[0] === ' ') count++;
    return recursiveCount(str.slice(1));
  }
}
```

This approach is better in that it keeps our function pure and doesn't expose the count, but it does lose the concise nature and readability of a contained recursive function.


## Debugger

VS Code has a useful debugging tool, indicated by the 'no bug entry' icon on the left hand side. It is an alternative to execution context diagrams, describing the state of the call stack and the local execution context.

It can be useful for recursive problems, as it can describe the individual parameters and return values for each invocation. This can make it easier to keep track of how the final answer is constructed.

To setup the debugger for use with Jest, see the [Jest Docs](https://jestjs.io/docs/troubleshooting#debugging-in-vs-code)

To use it, set **breakpoints** in your code by click the red dot to the right of a line number. This indicates that you want to 'break' the code at this point, and keep track of the thread yourself. A sensible place to add a breakpoint is on the first line inside the function.

Press the green play button at the top of the debug panel. This will the break code and show the current state of the execution context, as well as the call stack.

Using the debugger can feel like an art rather than a science sometimes. In the navigation at the top you had the opportunity to _step into_ or _step over_ your code. _Into_ will complete the next evaluation and move on to the next statement. Use _over_ when you want to move past an execution context, as you may well wish to do for code which is not your own (for example, `console.log` is a remarkably in-depth process).

Whilst in debugging mode, you may wish to pay attention to:

- the call stack, which will tell you about your current invocation and any previous invocations that are yet to resolve
- the variables in the local scope or current execution context, including computed return values
- when you hover your mouse over a variable in your code, you can see its current value at this point in the execution context
