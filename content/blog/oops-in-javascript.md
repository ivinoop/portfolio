---
title: "OOPs in JavaScript - Creating Objects And 'this' Keyword"
date: 2021-10-24T20:32:29+05:30
draft: false
description: "JavaScript is a flexible, object oriented language. This means that it allows developers to create different kinds of objects for different purposes."
tags: ["javascript"]
cover:
    image: "/blog/oops-in-javascript/oops-in-js.png"
---

JavaScript is a flexible, object oriented language. This means that it allows developers to create different  kinds of objects for different purposes. Almost everything in JavaScript (except Primitive types) is an object (Booleans, Strings, Numbers, Dates, Arrays, Functions, Objects, etc). 

## Creating Objects

Let us look at the different ways to create an object in JS. 

### 1. Object Literal

This is the most popular and easiest way of creating objects. Object literal consists of the type of variable/object name (let, var, or const), object name, and the collection of properties inside it. Here's an example to understand it better - 

```jsx
let obj = {}; // Object Literal
```
A more detailed object literal -

```jsx 
let userDetails = {
firstName: 'Arya',
lastName: 'Stark',
occupation: 'Girl With No Name',
};
```
### 2. Object Constructor

Constructors are special functions that are called when an object is created with the `new` keyword. Take a look at the example below - 

```js
let person = new Object({ 
name: 'Jon',
family: 'Targaryen',
occupation: 'Dragon Rider',
});
```
Here, the keyword `new` is used along with the case-sensitive keyword `Object`, indicating that it is a special keyword used in creating objects through Object Constructor method. 

The result is the same as creating object through Object Literals.

### 3. Object.create

This is another method to create new objects, which gives us more control over handling them. `Object.create` accepts a parameter, which can be either `null` or an object (key-value pairs).

```js
let user = Object.create({
name: 'Vinoo',
designation: 'Developer',
});
```
If we pass `null` as the parameter, it still creates an empty object. 

## The `this` keyword

In simple words, `this` is a special predefined variable that is present in every function declaration. The value of `this` variable changes according to the way we call the function in which it is defined. 

`this` is used in both global and function contexts. It always points to an object. More specifically, `this` references the object that is currently calling the function. 

Example - 

```js
const add5 = {
    a: 10;
    addition: function() {
    return this.a = this.a + 5;
    }
};

add5.addition();
```
Here, `addition()` is a function which is a property of the `add5` object (a function inside a property becomes a method). Hence, inside the `addition()` **method**, `this` references the `add5` object.

Whenever we call a function through an object ( e.g -`add5.addition()`), `this` will always point to that object. 

In Global context, `this` refers to the **global object**, which is the `window` object in a web browser. 

If we run the following snippet, it can be seen that`this` points to the `window` global object - 

```js
console.log(this === window); //true
```

Essentially, if a property is assigned to `this`, then that property is added to the global object, and can be accessed by the `window` object. 

Example - 

```js
this.value = 21;
console.log(window.value);
```

The output will be `21`.

---

`this` keyword has more use cases in the Function context, which we shall see in an upcoming post. 

Stay tuned! 🚀