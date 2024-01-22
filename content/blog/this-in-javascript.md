---
title: "this In Javascript - Simplified"
date: 2023-10-17T21:51:23+05:30
draft: false
description: "What is `this` keyword in JavaScript? How does it work in different scenarios?"
tags: ["javascript", "javascript-series"]
cover:
    image: "/blog/this-in-javascript/7.png"
---

> This is the 7th post in [The Complete JavaScript Series.](https://vinoo.in/tags/javascript-series/)

## Intro

Simply put, `this` is a special variable that is created for every Execution Context.

Once `this` has been created for the Execution Context (EC), it becomes a "keyword". Then, it points to (or takes the value of) the parent of whatever function has used `this`. The interesting thing about `this` is that it behaves differently with respect to how it is used and called in different use cases. Let us see them one by one.

## Different scenarios of 'this' -

### #1: Default Binding

A simple program that has nothing but this statement - `console.log(this)` - will show that `this` will point to the global window object, like so -

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697549151962/767ea147-5108-4d21-9d5f-3a793a253edd.png)

### #2: Implicit Binding

### 'this' in a method

> A method is any function that is defined inside an object.

In the example below, `this` will point to the parent object of the method that calls `this`. Since `getCurrentYear` is the method that calls `this`, the value of `this` will now point to the parent object - `details`. Hence, `this.futureYear` is equal to typing `details.futureYear`.

```javascript
const details = {
    name: 'Vinoo',
    futureYear: 2049,
    getCurrentYear: function () {
        console.log(2199 - this.futureYear)
        console.log(this)
    }
}

details.getCurrentYear() // 150
```

As we can see, we get the result `150` when the function is called. The value for `futureYear` was accessed through the `this` variable. The value of `this` itself is the `details` object -

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697547846530/dd976ad0-ab06-4961-9d5e-c9bd2474cea3.png)

### 'this' in a function call

In a regular function call with strict mode enabled, `this` will return `undefined` 👇

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697537702385/142583e6-7618-4e5e-a500-bc94751e1bf2.png)

Otherwise, `this` will point to the global `window` object 👇

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697537628068/5566f6fb-3141-4902-b7ee-698c2ec8188d.png)

### 'this' in Arrow Functions

In arrow functions, `this` simply points to the `this` of its surrounding function - which means `this` points to the lexical scope/parent scope. This goes to say that arrow functions do not get their own `this`.

Example -

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697538773579/5b6de135-c469-47ba-9cf1-3f4fac027b94.png)

### #3: Explicit Binding

As the name suggests, explicit binding is a way to explicitly bind the value of `this` to a specific object. There are 3 ways to do this -

1. `call()` - Let's say that our function is outside the object, and we still want `this` in the function to point to the object. We can do that as follows -
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697557277373/081f19e2-47b9-40f0-a043-308a6b92840e.png)
    
    Here, we are "binding" the object `details` to the function `displayName()` so that the `this` variable in that function can point to the desired object. If we DO NOT use the "call" binding, then as usual, `this` will simply point to the global window object.
    
    Now, what if we have multiple arguments to be passed? Look at the example below -
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697557761310/1217fe8c-d088-44bc-8bb4-64d66e62c92a.png)
    
    `call()` successfully binds the details we have passed to the `person` object and then the elements of the `hobbies` array, individually. But this is not the most efficient to bind things, is it? This is where the `apply()` method helps us.
    
2. `apply()` - this method works exactly like `call()` except that we can pass the array variable itself as a parameter instead of the array elements individually. Everything else stays the same -
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697558002910/1e754642-26a0-4ed8-b4b9-2eb7c0b6d6c5.png)
    
3. `bind()` - this method too, is almost no different from the `call()` or `apply()` methods but there is an important advantage to using it. First, let's look at an example -
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697558864224/4f1f70a4-9af3-45ca-8d6f-320f14b9b9d5.png)
    
    Notice how we assigned the `bind()` method to a variable `foo` and then called it as a function? This is because `bind()` returns a new function that we can then invoke anywhere we wish. Now for the advantage bit - `foo` does not even get instantiated anywhere, does not have a reference, nothing. Even though the `person` object does not have a property named `foo`, we can still explicitly bind `foo` to `person` much later in the program such that `person` will now recognise `foo` as its own method.
    

### #4: 'new' Binding

`new` is yet another special keyword that is exclusively used in the case of constructor functions. These are used to create new objects. Let's look at this example -

```javascript
let Wizard = function(name, spell) {
    this.name = name
    this.spell = spell
    this.castMagic = function() {
        console.log(this.name + ' has cast this spell - ' + this.spell)
    }
}

let hermione = new Wizard('Hermione', 'Occulus Reparo')
let ron = new Wizard('Ron', 'Sunshine Daisies')
```

`new` is used to denote that we are creating a new object (instantiating) for the `Wizard` function. Note that `Wizard` is capitalised when we use `new` - a simple convention to differentiate, and also specify that `this` variable will always point to the empty object created by `new`. This is how we can get new instances easily for the same function with different arguments. Just simple Object Oriented Programming in action 😎

We will study more about constructors and classes in depth in a later post.

### #5: HTML Event Element Binding - 'this' in Event Listeners

In case of event listeners, `this` points to the DOM element that the handler is attached to. Example -

```javascript
sendBtn.addEventListener('click', function () {
  console.log(2 + 3)
  console.log('Value of this - ', this)
})
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697545354354/10117b9a-1276-407c-8a11-12dea72fae6c.png)

Here, on clicking the button 'Send', the value of `this` will indeed be the DOM element `button` that the function/handler is attached to.

**NOTE -** if this function/handler is an arrow function, then the value of `this` again defaults to the global window object, like so -

```javascript
sendBtn.addEventListener('click', () => {
  console.log(2 + 3)
  console.log('Value of this - ', this)
})
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697546026030/2941d979-6b35-42e3-af49-15d22dae82ad.png)

---

## Closing Notes

That was it for `this`!

![Season 2 Joey GIF by Friends](https://media0.giphy.com/media/RfAuCCsCMVJLgPX88v/giphy.gif?cid=ecf05e477ncktpkmi43dgptfl96fba12nh6sqt3uscy7k3nd&ep=v1_gifs_search&rid=giphy.gif&ct=g)

We will study more about how `this` is important for classes, constructors, and object oriented programming in the later posts.

Until the next one!

Keep shipping 🚀