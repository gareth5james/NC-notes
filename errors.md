# Understanding Errors

## Learning Objectives

- know the most common JavaScript error types
- know how to read JavaScript error messages

## Making errors less overwhelming

When first learning to code, it is entirely natural to feel overwhelmed or panicked when you come across errors.

Getting errors does _not_ mean that you are doing badly - every senior developer out there will encounter errors on a daily basis and you will almost certainly see errors throughout your entire coding experience. The important thing is learning how to _approach_ errors in a constructive way. When approached with patience and logic, errors are incredibly useful.

## Common JS Error Types

There are [lots of different types of errors you will encounter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error), but there are three that are particularly common when starting to learn JavaScript.

### SyntaxErrors

The MDN definition of a [SyntaxError](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SyntaxError) is as follows:

> The SyntaxError object represents an error when trying to interpret syntactically invalid code. It is thrown when the JavaScript engine encounters tokens or token order that does not conform to the syntax of the language when parsing code.

In other words, you have to write your code in such a way that it does not break any of the rules of how JavaScript should be constructed. For example, one such rule is that a function should be invoked by using both opening and closing parenthesis - if you miss out the closing parenthesis this breaks the JS syntax.

### ReferenceError

ReferenceError occur when referencing an "invalid" reference. A variable could be classed as "invalid" because it is _non-existent_, or because it doesn't exist _yet_. Another reason could be it isn't accessible from the area of code we are trying to reference it; this is known as the variable is not within 'scope'.

### TypeError

MDN defines a [TypeError](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypeError) as occurring "when an operation could not be performed, typically (but not exclusively) when a value is not of the expected type."

Often, this is because we are _using_ data wrong considering its _datatype_, and so the action cannot be performed.

## Anatomy of an error

On top of the error type giving us clues for what might have gone wrong, there are other pieces of information often contained in error messages. Usually, you will also see:

- the file that the error is coming from (this is particularly useful when working on larger projects),
- the line number,
- the specific character position in that line,
- a more specific message about what is happening, and
- a stack trace (though this will be most useful when in larger projects)

You can (and should!) use this variety of information as part of your debugging process. With this, you will know roughly where the error is coming from and roughly what could be causing it, before even looking at your code.
