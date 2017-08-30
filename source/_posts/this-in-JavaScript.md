---
title: '"this" in JavaScript'
date: 2017-08-29 23:53:24
updated: 2017-08-30 14:15:00
tags: javascript
---
We will talk about `this` in JavaScript.

We also mention the difference between the way JS looks up a variable as a functional programming language and the way JS finds where the `this` keyword refers to.

# Global context
`this` refers to global object, whether in [_strict mode_](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode) or not.
```javascript
console.log(this === window) // true

a = 37;
console.log(window.a); // 37
console.log(this.a) // 37
console.log(a) // 37
```
> **Strict mode**
> * To invoke strict mode in a scope: `'use strict'`.
> * In strict mode, we use a restricted variant of JavaScript. It's **not** a subset of JavaScript and it intentionally has different semantics from normal one.
> * Do **not** rely on strict mode, some browsers may not support.
> * We only talk about non-strict mode (sloppy mode).

<br \>
# Function context
`this` depends on how the function is called.
```javascript
function f1() {
  return this;
}
// In a browser:
f1() === window; // the window is the global object in browsers
// In Node:
f1() === global;
```
In function context, `this` refers to the object within which the function is called. In other words, **`this` refers to the object in its execution environment**.

> `this` does **not** refer to the object where the function is **defined**. What `this` refers to is determined in **runtime**, not **compile time** (we can know where function is defined in compile time).


```javascript
var name = 'foo';

var a = {
  name: 'bar',
  getName: function () {
    return name;
  },
  getThisName: function () {
    return this.name;
  },
  getThis: function () {
    return this;
  }
};

a.getName(); // foo
a.getThisName(); // bar
a.getThis() === a; // true
```
In functional programming, when we access variable `name` with `a.getName()`, JS looks up the scope chain<sup>[1](#superscript)</sup> (the current scope is global scope) and then finds `name = 'foo'`, so `"foo"` is returned.
However, when we access `this` with `a.getThisName()`, it finds variable/object `a` because `getThisName()` is defined in `a` (same for `getThis()`).

```javascript
var f = a.getThis;
var g = a.getThisName;
f() === window; // true
g(); // foo
```
Variable `a.getThis` is passed<sup>[2](#superscript)</sup> to variable `f` and `f` is called in global context (execution environment is global environment), so `this` refers to `global`.

---
> <a name="superscript"></a> [1] JavaScript uses static scope.
> [2] JavaScript passes variable by sharing object.

<p></p>

> **Reference:**
> https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this
> https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode
