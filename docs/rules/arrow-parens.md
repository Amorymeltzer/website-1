---
title: Rule arrow-parens
layout: doc
---
<!-- Note: No pull requests accepted for this file. See README.md in the root directory for details. -->
# Require parens in arrow function arguments (arrow-parens)

Arrow functions can omit parentheses when they have exactly one parameter. In all other cases the parameter(s) must
be wrapped in parentheses. This rule enforces the consistent use of parentheses in arrow functions.

## Rule Details

This rule enforces parentheses around arrow function parameters regardless of arity. For example:

```js
// Bad
a => {}

// Good
(a) => {}
```

Following this style will help you find arrow functions (`=>`) which may be mistakenly included in a condition
when a comparison such as `>=` was the intent.


```js
// Bad
if (a => 2) {
}

// Good
if (a >= 2) {
}
```

The rule can also be configured to discourage the use of parens when they are not required:

```js
// Bad
(a) => {}

// Good
a => {}
```

### Options

The rule takes one option, a string, which could be either "always" or "as-needed". The default is "always".

You can set the option in configuration like this:

"arrow-parens": [2, "always"]

#### "always"

When the rule is set to `"always"` the following patterns are considered warnings:

```js
/*eslint arrow-parens: [2, "always"]*/

a => {};                     /*error Expected parentheses around arrow function argument.*/
a => a;                      /*error Expected parentheses around arrow function argument.*/
a => {'\n'};                 /*error Expected parentheses around arrow function argument.*/
a.then(foo => {});           /*error Expected parentheses around arrow function argument.*/
a.then(foo => a);            /*error Expected parentheses around arrow function argument.*/
a(foo => { if (true) {}; }); /*error Expected parentheses around arrow function argument.*/
```

The following patterns are not warnings:

```js
/*eslint arrow-parens: [2, "always"]*/

() => {};
(a) => {};
(a) => a;
(a) => {'\n'}
a.then((foo) => {});
a.then((foo) => { if (true) {}; });
```

##### If Statements

One benefits of this option is that it prevents the incorrect use of arrow functions in conditionals:

```js
var a = 1;
var b = 2;
// ...
if (a => b) {
 console.log('bigger');
} else {
 console.log('smaller');
};
// outputs 'bigger', not smaller as expected
```

The contents of the `if` statement is an arrow function, not a comparison.
If the arrow function is intentional, it should be wrapped in parens to remove ambiguity.

```js
var a = 1;
var b = 0;
// ...
if ((a) => b) {
 console.log('truthy value returned');
} else {
 console.log('falsey value returned');
};
// outputs 'falsey value returned'
```

The following is another example of this behavior:

```js
var a = 1, b = 2, c = 3, d = 4;
var f = a => b ? c: d;
// f = ?
```

`f` is an arrow function which takes `a` as an argument and returns the result of `b ? c: d`.

This should be rewritten like so:

```js
var a = 1, b = 2, c = 3, d = 4;
var f = (a) => b ? c: d;
```


#### "as-needed"

When the rule is set to `"as-needed"` the following patterns are considered warnings:

```js
/*eslint arrow-parens: [2, "as-needed"]*/

(a) => {};                     /*error Unexpected parentheses around single function argument*/
(a) => a;                      /*error Unexpected parentheses around single function argument*/
(a) => {'\n'};                 /*error Unexpected parentheses around single function argument*/
a.then((foo) => {});           /*error Unexpected parentheses around single function argument*/
a.then((foo) => a);            /*error Unexpected parentheses around single function argument*/
a((foo) => { if (true) {}; }); /*error Unexpected parentheses around single function argument*/
```

The following patterns are not warnings:

```js
/*eslint arrow-parens: [2, "as-needed"]*/

() => {};
a => {};
a => a;
a => {'\n'};
a.then(foo => {});
a.then(foo => { if (true) {}; });
(a, b, c) => a;
(a = 10) => a;
([a, b]) => a;
({a, b}) => a;
```

## Version

This rule was introduced in ESLint 1.0.0-rc-1.

## Resources

* [Rule source](https://github.com/eslint/eslint/tree/master/lib/rules/arrow-parens.js)
* [Documentation source](https://github.com/eslint/eslint/tree/master/docs/rules/arrow-parens.md)