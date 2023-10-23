---
title: "What is TypeScript"
description: "What is TypeScript"
---


## #1 Goal for this course

By the end of this course, **I want you to have a rock solid mental model, that will serve you well for years.**


<br>

## What is TypeScript?
TypeScript is an open source, typed **syntactic superset** of JavaScript
+ Compiles to readable JS
+ Three parts: Language, Language Server and Compiler
+ Kind of like a fancy linter

<br>

## Typescript Is Increasingly Popular
![TypeScript Popularity](./images/graph.png)


<br>

## Why developers want types

It allows you, as a code author, **to leave more of your intent “on the page”**

<br>

This kind of intent is often missing from JS code. For example:

```js
function add(a, b) {
  return a + b
}
```

Is this meant to take numbers as args? strings? both?

<br>

What if someone who interpreted a and b as numbers made this “backwards-compatible change?”


```js
function add(a, b, c = 0) {
  return a + b + c
}
```


We’re headed for trouble if we decided to pass strings in for a and b!

<br/>


Types make the author’s intent more clear


```ts
function add(a: number, b: number): number {
  return a + b
}
add(3, "4")
```

### **It has the potential to move some kinds of errors from runtime to compile time**

<br>

**Examples:**

+ Values that are potentially absent (null or undefined)
+ Incomplete refactoring
+ Breakage around internal code contracts (e.g., an argument becomes required)

<br>

### **It serves as the foundation for a great code authoring experience**

<br>

**Example:** in-editor autocomplete,

