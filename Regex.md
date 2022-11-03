# Regular expressions

## Learning objectives

- understand how regular expressions can be used to match patterns in text
- know the basic structure of a regular expression
- know how to create a regular expression in JavaScript
- know how to test regular expressions in JavaScript

## Regex

A regular expression or _regex_ is a pattern which is designed to find matches within a body of text. They are frequently used in search engines, word processors and online forms.

In JavaScript, a regex pattern is denoted with an opening and closing forward slash `/`. For example `/i'm a js regex!/`

## Literal Characters

We can use literal characters when we are sure that a specific character needs to appear in the pattern. For example, if we want to test some text to determine whether it had the characters `dog` in that order, we could construct the following regex.

```js
/dog/  // our regex to find the dog 🐕
"dog" ✅ //we have a match!
"catcatcatdogcatcatcat" ✅ //also a match
"DOG" ❌ //the regexes are by default case-sensitve so this wouldn't match
"dgo" ❌ //we have the right characters, but they are not in the right order.
```

## Meta-characters

Meta-characters are not interpreted literally. They are used to give the regex the ability to match more complex patterns rather than a string of literal characters. We can use square brackets to wrap a list of characters we are looking for. Unlike the previous example, using a character set, `/[dog]/` will match any _single_ character found with the `[]`.

We can be less specific about the characters we are looking for as well:

**\d** - matches any numerical character.

**\w** - matches any alphanumerical character as well as underscores.

**\s** - matches whitespace characters.

If we want match patterns based on a certain location in the text, we can make use of **anchors**:

**^** - matches the beginning of the text.

**\$** - matches the end of the text.

To use a meta-character as a literal matcher, prepend it with a `\` eg. `\$`

## Quantifiers

We can be specific about the number of characters we are interested in.

`[abc]{3}` would match exactly three consecutive characters from within the character set. eg. aab, bbb, or abc.

`[abc]{3,6}` would match the same characters but the length can be between 3-6 characters long. _NB: regex will be greedy by default and match the largest pattern if it can._

**+** checks there are _one or more_ within the text. **\*** is similar but works on the basis there is _zero or more_ matches.
eg.

```js
/[abc]+!*/; // matches one or more of the characters in the character set followed by zero or more exclamation marks.
```

## Flags

A regex is commonly appended with a series of letters. These are known as _flags_ and alter the behaviour of how the regex works. They can be chained together (but be warned, some flags override the behaviours of others). eg. `/[a-z]/gi`
Here is a list of some commonly used ones:

**g** - global flag. Tells the regex to check the whole passage of text for matches. Without this flag, the regex would only pick up the first match it comes across.

**i** - case insensitive flag. With this flag activated, the regex will not differentiate between upper and lower case characters.

**m** - multiline flag. This changes the behaviour of the start, `^` and end `$` anchors. By default they match the beginning and end of the entire body of text. With the multiline flag set to true the regex will consider each line to have a start and end.

## Regular Expressions in JavaScript

### Creating regular expressions

There are two ways to create a regular expression in JavaScript, either using a regular expression literal, or the `RegExp` constructor:

```js
const literalRegEx = /[abc]+!*/;
const constructorRegex = new RegExp('[abc]+!*');
```

### Using Regular Expressions

Regular expressions are most commonly used with the RegExp methods `test()` and `exec()`, and with the String methods `match()`, `replace()`, `search()`, and `split()`.

#### RegExp.test()

The [`.test()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/test) method is chained to a regular expression and invoked with the string to test. It will return `true` or `false` based on whether the string matches

```js
const str = 'table football';
const regex = /foo*/;
const result = regex.test(str); // true
```

#### String.match()

The [`.match()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/match) method is chained to a string and invoked with the regular expression to match. It will return matching results in an array. The `g` flag will determine wither all matching results are included, or only the first result.

```js
const paragraph = 'The quick brown fox jumps over the lazy dog. It barked.';
const regex = /[A-Z]/g; // the `g` flag will return all matching results, if omitted, only the first result will be returned
const found = paragraph.match(regex);

console.log(found);
// expected output: Array ["T", "I"] i.e. both matching uppercase characters between A-Z
```
