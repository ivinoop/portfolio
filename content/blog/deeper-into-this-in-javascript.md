---
title: "Deeper Into This in Javascript"
date: 2021-11-18T20:45:56+05:30
draft: false
description: "Let's dive deeper into different bindings of this that we will encounter when dealing with it in functions."
tags: ["javascript"]
cover:
    image: "/blog/deeper-into-this-in-js/deeper-into-this.png"
---

> This article was originally published [here](https://dev.to/vinoo/deeper-into-this-in-javascript-4lil).

In a [previous article](https://vinoo.hashnode.dev/oops-in-javascript-creating-objects-and-this-keyword), we saw how to use `this` keyword with objects. In this post, we shall dive deeper into different bindings of `this` that we will encounter when dealing with it in functions. Bindings mean the different ways `this` behaves in different contexts in a function. 

### 1. Default Binding

Consider the following example - 

```js
function defaultThis() {
 console.log(this);
 alert(`Welcome ${this.username}`);
}

defaultThis();
```
Since there is no `username` variable declared or defined, `this` keyword gets the default binding - it references the global `Window` object here, as can be seen below -

![this-1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1637222838694/oSoFc8ivU.png)


### 2. Implicit Binding

This binding is created by the behaviour of the function. Let's take an example to understand - 

```js
let hobbit = {
  name: 'Bilbo',
  welcome() {
    alert(`Hello ` + this.name);
  }
} 

hobbit.welcome();
```
The output would be as exptected - 


![this-2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1637222861638/kYN-SPTuu.png)

Here, since there is an object that calls the function `welcome()`, `this` implicitly refers to the object inside the function. 

### 3. Explicit Binding

Explicit binding means to explicitly bind the value of `this` to any specific object. 

There are 3 methods to implement explicit binding - 

* `call()`

Consider the code snippet we used above in Implicit Binding - the property `name` and method `welcome` are both defined inside the object `hobbit`. This makes the binding for `this` fairly..implicit 🌝. What if the object is separate from a method? Consider the snippet below -

```js
function welcome() {
  alert(`Welcome ${this.name}`);
}

let hobbit = {
  name: 'Frodo'
}

welcome(); // Welcome
welcome.call(hobbit); // Welcome Frodo
```

The first function call `welcome()` has no reference to an object, so it would not return anything in the alert statement after `Welcome`. 

The second function call is where we have accessed the object with the `call` method. This means that we are specifying to the browser to assign the object `hobbit` being passed as parameter to `this` using `call` method.

Another use case for `call` is that we can pass parameters to signify the value for `this` along with arguments for the function. Example - 

```js
function foo(spellOne, spellTwo) {
  alert(`${this.name} cast the spells ${spellOne} and ${spellTwo}`);
}

let wizard = {
  name: 'Ron Weasley'
};

foo.call(wizard, 'Expelliarmus', 'Slugulus Eructo');
```
Here, the function `foo` is called with the `call` method and the object `wizard` is passed as the first argument which automatically gets assigned to `this` in the function, along with the rest of the arguments. Note that the first argument always gets assigned to `this`.

The output is as below -


![this-3.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1637222884907/nVbiRBknt.png)


But there is a drawback for this use case. What if there are tens of arguments to be passed for multiple objects? Very cumbersome 😕 We have the next binding method to improve usability a little better. 

* `apply()`

Take a look at this snippet -

```js
function foo(spellOne, spellTwo) {
  alert(`${this.name} cast the spells ${spellOne} and ${spellTwo}`);
}

let wizard = {
  name: 'Ron Weasley'
};

foo.apply(wizard, ['Expelliarmus', 'Slugulus Eructo']);
```
The format is the same, except that instead of `call`, we use the method `apply`, and instead of passing the arguments one after the other, we just wrap them in an array. The output remains the same. 

* `bind()`

The `bind()` method creates a new function which when invoked, assigns the provided values to `this`.

Take a look at the snippet below - 

```js
function foo(spellOne, spellTwo) {
  alert(`${this.name} cast the spells ${spellOne} and ${spellTwo}`);
}

let wizard = {
  name: 'Ron Weasley'
};

let castSpell = foo.bind(wizard, 'Expelliarmus', 'Slugulus Eructo');

castSpell();
```

Here, we are using `bind()` to be referenced by the variable `castSpell`, which can then be invoked as a normal function call. 

The advantages of using `bind()` are that -

- We are explicitly binding the `foo()` method to the instance `castSpell` such that `this` of `foo()` is now bound to `castSpell`
- Even though the `wizard` object does not have `castSpell` as its property, because we are using `bind()`, `wizard` now recognises `castSpell` as its method

`bind()` returns a new function reference that we can call anytime we want in future. 

### 4. new Binding

`new` binding is used specifically for constructor functions. Take a look below -

```js
function Wizard(name, spell) {
  this.name = name;
  this.spell = spell;
  this.intro = function() {
    if(this.name === 'Hermione') {
    alert(`The witch ${this.name} cast the spell ${this.spell}`);
    } else {
    alert(`The wizard ${this.name} cast the spell ${this.spell}`);
    } 
  }
}

let hermione = new Wizard('Hermione', 'Occulus Reparo');
let ronald = new Wizard('Ronald', 'Slugulus Erecto');
```

Constructor functions are special functions that are used to create new objects. The use of `new` keyword means that we are creating a new object (or instance) of the (constructor) function.

Whenever `new` is used before any constructor function (name with the Capitalized convention followed), the JS engine undertands that `this` inside the function will always point to the empty object created by `new`. 

### 5. HTML Element Event Binding

`this` can be used to bind the values of specific events or elements in HTML.

Take a look at this example -

```html
<button 
class ="this-one"
onclick="console.log(this)">
this One
</button>
```

In this case, `this` will always bind itself to the element where the event happened; in this case, the `this-one` class button.

The output will be as below -


![this-4.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1637222914862/KFe13wniA.png)

Now take a look at this snippet -

```html
<button 
class ="this-two"
onclick="this.style.backgroundColor='orange'">
this Two
</button>
```
Here, `this` is again bound to the button with the class `this-two`, and the `onclick` event happens only on that specific button.

Output - 


![this-5.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1637222936687/c4q8eYO9U.png)

How about when we call a function within the element?

```js
<button 
class ="this-three"
onclick="changeColor()">
this Three
</button>

<script>
  function changeColor() {
    console.log(this);
  }
</script>
```

Note that we are calling the `console.log()` function along with `this`.

So, the value of `this` is as below -


![this-6.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1637222948634/FRdahk9ed.png)

Here, `this` points to the global `Window` object. We can see that Default Binding occurs here since the function `changeColor()` is called without a prefix.

---

`this` is definitely strange. However, the use cases provide us with flexibility to use objects effectively. 
