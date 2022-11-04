# HTML and CSS

## Learning objectives

- understand the different purposes of HTML and CSS in creating a web page
- know how to identify different elements within a Web page
- understand how to use the following HTML tags: `main`, `h1`, `h2`, `p`, `ul`, `ol`, `li`, `a`
- understand how to use the following CSS properties: `margin`, `padding`, `border`, `border-radius`, `font-family`, `font-size`, `background-color`, `color`, `text-align`

## What is HTML?

HyperText Markup Language, or **HTML**, is the standard markup language for creating Web pages. A *markup language* uses tags to identify structure and elements within a document. Each element in an HTML document tells the browser how to display the contents of that document.

> [MDN: HTML](https://developer.mozilla.org/en-US/docs/Web/HTML)

## HTML tags

HTML labels its elements with tags, which are identified by the angle brackets around them. Most HTML elements have a start tag, followed by content, and then an end tag:

```html
<p>I am a paragraph!</p>
```

Some HTML tags are *self-closing*. They end with `/>` instead of just `>`:

```html
<img src="cool-image.png" alt="Cool." />
```

## semantic HTML vs non-semantic HTML

Some HTML tag names describe the content that's inside them. We call these **semantic** tags - tags such as `<article>`, `<header>` and `<form>`. Other tags are more generic, and neither the developer nor the browser can tell what the content is from the tag name - these are **non-semantic** tags, such as `<div>` and `<span>`.

Using semantic HTML elements wherever possible has a lot of benefits for both users and developers. For example, using semantic elements makes it easier for search engines to make sense of the page content, so that its search results are more relevant. Semantic HTML is also incredibly useful for accessibility purposes; screen readers and assistive technologies can tell which elements have the content that's most useful for the user.

> [CSS Tricks: Why, How and When to Use Semantic HTML and ARIA](https://css-tricks.com/why-how-and-when-to-use-semantic-html-and-aria/)

## What is CSS?

Cascading Style Sheets, or **CSS**, is a *style sheet language* that expresses the presentation of structured documents. A CSS file tells the browser how to display elements within the HTML document that links to it.

> [MDN: CSS](https://developer.mozilla.org/en-US/docs/Web/CSS)

## CSS rulesets

A CSS ruleset defines the styling for a selected element or group of elements. Take a look at this example:

```css
p {
  color: red;
}
```

The name of the element at the beginning of the ruleset, in this case `p`, is the **selector**. All `<p>` tags in the HTML document will be targeted by this ruleset. The lines inside the braces that come after the selector are called **declarations**. In this particular ruleset, we want to affect the `color` **property**, and the **property value** we are assigning to it is `red`. You can have as many declarations inside the ruleset as you like.

CSS syntax has some rules to follow:

- the declaration has to be wrapped in braces `{}`
- separate the property from its value with a colon `:`
- separate each declaration with a semicolon `;` (semicolons aren't optional in CSS in the way that they're optional in JavaScript)

## the cascade

The order in which you write your CSS rules matters. When two rulesets are applied to an element, the last one to appear in the CSS file will be the ruleset that's applied. This is called the *cascade*, the C in CSS. In the following example:

```css
p {
  color: red;
}

p {
  color: blue;
}
```

...the colour of the text in every `p` tag in the HTML will be **blue**.

> [MDN: Introducing the CSS Cascade](https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade)

## CSS resets

Each browser - Chrome, Firefox, Edge and so on - will apply default styling to an HTML document if there is no CSS provided. However, default styling differs slightly from browser to browser. In order to have full control over our own CSS styles, so that our styles look as identical as possible across all browsers, it's a good idea to use **CSS resets**. This is a basic example of a reset, in which the margin and padding of all elements, denoted by the wildcard `*`, are set to 0:

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
```

Because of the cascade, resets are always the first styles to appear at the top of a CSS file. That way, if we want to have different styles in a particular element, the later property will *override* the property in the reset.

## Further reading

- [CSS Tricks](https://css-tricks.com/)
- [CSS Battle](https://cssbattle.dev/)
- [CSS Grid Garden](https://cssgridgarden.com/)
- [CSS Flexbox Froggy](https://flexboxfroggy.com/)
